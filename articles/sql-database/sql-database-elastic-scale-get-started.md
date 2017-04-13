<properties 
    pageTitle="Γρήγορα αποτελέσματα με εργαλεία ελαστικότητας βάσης δεδομένων" 
    description="Βασική επεξήγηση της δυνατότητας εργαλεία ελαστικότητας βάσης δεδομένων της βάσης δεδομένων SQL Azure, συμπεριλαμβανομένων των εύκολη για να εκτελέσετε το δείγμα εφαρμογής." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Γρήγορα αποτελέσματα με εργαλεία ελαστικότητας βάσης δεδομένων

Αυτό το έγγραφο παρουσιάζει την εμπειρία για προγραμματιστές, εκτελώντας την εφαρμογή δείγματος. Το δείγμα δημιουργεί μια απλή εφαρμογή sharded και εξετάζει βασικές δυνατότητες του εργαλεία ελαστικότητας βάσης δεδομένων. Το δείγμα παρουσιάζει συναρτήσεις της [βιβλιοθήκης ελαστικότητας βάση δεδομένων προγράμματος-πελάτη](sql-database-elastic-database-client-library.md)

Για να εγκαταστήσετε τη βιβλιοθήκη, μεταβείτε στο [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Σημειώστε ότι η βιβλιοθήκη έχει εγκατασταθεί με την εφαρμογή δείγματος που περιγράφεται παρακάτω.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

1. Απαιτείται το Visual Studio 2012 ή νεότερη έκδοση με C#. Κάντε λήψη μιας δωρεάν έκδοσης στο [Visual Studio λήψεις](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. Nuget 2.7 ή νεότερη έκδοση. Για να λάβετε την πιο πρόσφατη έκδοση, ανατρέξτε στο θέμα [NuGet κατά την εγκατάσταση](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Κάντε λήψη και εκτελέστε την εφαρμογή δείγματος

Το **ελαστικά βάσης δεδομένων με Azure SQL — γρήγορα αποτελέσματα** δείγμα εφαρμογής απεικονίζει τις πιο σημαντικές πλευρές της εμπειρίας ανάπτυξης sharded εφαρμογών χρησιμοποιώντας εργαλεία ελαστικότητας βάσης δεδομένων Azure SQL. Εστιάζει στη περιπτώσεις χρήσης κλειδιού για [shard χάρτης διαχείρισης](sql-database-elastic-scale-shard-map-management.md), [εξαρτώμενα δρομολόγηση δεδομένων](sql-database-elastic-scale-data-dependent-routing.md) και [Υποβολή ερωτημάτων πολλών shard](sql-database-elastic-scale-multishard-querying.md). Για να κάνετε λήψη και να εκτελέσετε το δείγμα, ακολουθήστε τα παρακάτω βήματα: 

1. Ανοίξτε το Visual Studio και επιλέξτε **Αρχείο -> Δημιουργία -> έργου**.
2. Στο παράθυρο διαλόγου, κάντε κλικ στην επιλογή **Online**.

    ![Νέο έργο > Online][2]
3. Στη συνέχεια, κάντε κλικ στην επιλογή **Visual C#** στην περιοχή **δείγματα**.

    ![Κάντε κλικ στην επιλογή Visual C#][3]
4. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **ελαστικότητας db** για να αναζητήσετε το δείγμα. Ο τίτλος εμφανίζεται **Ελαστικότητας εργαλεία DB για SQL Azure - γρήγορα αποτελέσματα** .

    ![Πλαίσιο αναζήτησης][1]
 
5. Επιλέξτε το δείγμα, επιλέξτε ένα όνομα και μια θέση για το νέο έργο και πατήστε **OK** για να δημιουργήσετε το έργο.
6. Ανοίξτε το αρχείο **app.config** στη λύση για το έργο δείγμα και ακολουθήστε τις οδηγίες στο αρχείο για να προσθέσετε το όνομα του διακομιστή βάσης δεδομένων Azure SQL και τις πληροφορίες σύνδεσης (όνομα χρήστη και κωδικός πρόσβασης).
7. Δημιουργία και εκτέλεση της εφαρμογής. Όταν σας ζητηθεί, επιτρέψτε Visual Studio για να επαναφέρετε τα πακέτα NuGet της λύσης. Αυτό θα κάνετε λήψη την πιο πρόσφατη έκδοση της βιβλιοθήκης ελαστικότητας βάση δεδομένων προγράμματος-πελάτη από NuGet.
8. Αναπαραγωγή με τις διαφορετικές επιλογές για να μάθετε περισσότερα σχετικά με τις δυνατότητες της βιβλιοθήκης προγράμματος-πελάτη. Σημείωση τα βήματα της εφαρμογής σας μεταφέρει στην κονσόλα εξόδου και μην διστάσεις να εξερευνήσετε τον κώδικα στο παρασκήνιο.

    ![Πρόοδος][4]

Συγχαρητήρια, έχετε δημιουργηθεί και εκτελέστε την πρώτη εφαρμογή sharded χρησιμοποιώντας εργαλεία ελαστικότητας βάσης δεδομένων στη βάση δεδομένων SQL Azure με επιτυχία. Ρίξτε μια γρήγορη ματιά το shards που δημιουργήθηκε το δείγμα με σύνδεση στο Visual Studio ή SQL Server Management Studio στο διακομιστή DB Azure. Θα παρατηρήσετε νέες βάσεις δεδομένων shard δείγμα και μια βάση δεδομένων shard χάρτη manager που έχουν δημιουργηθεί από το δείγμα.

> [AZURE.IMPORTANT] Συνιστάται να χρησιμοποιείτε πάντα την πιο πρόσφατη έκδοση του Management Studio για να παραμείνετε συγχρονισμένοι με ενημερώσεις για το Microsoft Azure και βάση δεδομένων SQL. [Ενημέρωση SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>Πλήκτρο τμήματα του δείγματος κώδικα

1. **Διαχείριση Shards και Shard χάρτες**: Ο κώδικας παρουσιάζει τον τρόπο εργασίας με shards, περιοχές, και αντιστοιχίσεις στο αρχείο **ShardMapManagerSample.cs**. Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με αυτό το θέμα εδώ: [Shard χάρτης διαχείρισης](http://go.microsoft.com/?linkid=9862595).  
2. **Εξαρτώμενα δρομολόγηση δεδομένων**: δρομολόγηση των συναλλαγών το σωστό shard εμφανίζονται στο **DataDependentRoutingSample.cs**. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Δρομολόγηση εξαρτώμενα δεδομένων](http://go.microsoft.com/?linkid=9862596). 
3. **Ερωτήματα μέσω πολλών Shards**: ερωτήματα σε shards απεικονίζεται στο αρχείο **MultiShardQuerySample.cs**. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Υποβολή ερωτημάτων πολλών Shard](http://go.microsoft.com/?linkid=9862597).
4. **Προσθήκη κενή shards**: επαναληπτικού Προσθήκη νέα κενή shards πραγματοποιείται από τον κώδικα στο αρχείο **AddNewShardsSample.cs**. Λεπτομέρειες αυτού του θέματος καλύπτονται εδώ: [Shard χάρτης διαχείρισης](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Άλλες λειτουργίες ελαστικότητας κλίμακας

1. **Η διαίρεση ενός υπάρχοντος shard**: τη δυνατότητα να διαιρέσετε shards παρέχεται μέσω του **εργαλείου διαίρεσης συγχώνευσης**. Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με αυτό το εργαλείο εδώ: [Επισκόπηση εργαλείων διαίρεσης συγχώνευσης](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Συγχώνευση υπάρχουσες shards**: Shard συγχωνεύσεις εκτελούνται επίσης με το **εργαλείο διαίρεσης συγχώνευσης**. Για περισσότερες πληροφορίες, ανατρέξτε στην: [Επισκόπηση εργαλείων διαίρεσης συγχώνευσης](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Κόστος

Τα εργαλεία ελαστικότητας βάσης δεδομένων είναι δωρεάν. Εργαλεία ελαστικότητας βάσης δεδομένων δεν επιβάλλει επιπλέον χρεώσεις επάνω από το κόστος για την χρήση του Azure. 

Για παράδειγμα, το δείγμα εφαρμογής δημιουργεί νέες βάσεις δεδομένων. Το κόστος εξαρτάται από την έκδοση βάσης δεδομένων SQL Azure DB που επιλέγετε και το Azure χρήσης της εφαρμογής σας.

Για πληροφορίες για τις τιμές ανατρέξτε [Λεπτομέρειες τιμολόγησης βάση δεδομένων SQL](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες πληροφορίες σχετικά με τα εργαλεία ελαστικότητας βάσης δεδομένων, ανατρέξτε στα θέματα:

* [Χάρτης τεκμηρίωση εργαλεία ελαστικότητας βάσης δεδομένων](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Δείγματα κώδικα: 
    -    [Ελαστικά DB με Azure SQL - γρήγορα αποτελέσματα](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Ελαστικά DB με Azure SQL - ενοποίηση με το πλαίσιο οντότητα](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Ελαστικότητα shard στο Κέντρο δέσμης ενεργειών](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Ιστολόγιο: [ανακοίνωση ελαστικότητας κλίμακας](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Κανάλι 9: [ελαστικά κλίμακα Επισκόπηση βίντεο](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Φόρουμ συζήτησης: [φόρουμ βάση δεδομένων SQL του Azure](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Για τη μέτρηση της απόδοσης: [μετρητές επιδόσεων για διαχείριση shard χάρτη](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
