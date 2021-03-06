<properties 
    pageTitle="Πώς να συντάξετε ερωτήματα στην ανάλυση ροή | Microsoft Azure" 
    description="Σύνταξη ερωτημάτων στο ροή ανάλυση και δεδομένα ερωτήματος | εκμάθηση τμήμα διαδρομής."
    keywords="τον τρόπο σύνταξης ερωτημάτων, ερωτήματος δεδομένων, να συντάξετε ένα ερώτημα, η σύνταξη ερωτημάτων"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="how-to-write-queries-in-stream-analytics"></a>Πώς να συντάξετε ερωτήματα στην ανάλυση ροής

Σύνταξη ερωτημάτων για επεξεργασία λογικής στο Azure ροή ανάλυση ροή έχει υλοποιηθεί ως "μόνιμη ερωτήματος", που έχει οριστεί για μια εργασία ξεκινά και να εκτελεστεί σε δεδομένα, όπως να φτάσει η εργασία. Το μετασχηματισμό δεδομένων εκφράζεται σε μια γλώσσα ερωτημάτων μοιάζει με SQL, που είναι σε μεγάλο βαθμό ένα υποσύνολο T-SQL με ορισμένες επεκτάσεις γλώσσα που προστέθηκε όπως [Άνοιγμα παραθύρου](https://msdn.microsoft.com/library/azure/dn835019.aspx) που χρησιμοποιείται για να εκφράσετε χρονικό σημασιολογία.

## <a name="writing-queries"></a>Η σύνταξη ερωτημάτων: ##

1. Στην εργασία σας ανάλυση ροή στην πύλη διαχείρισης Azure, κάντε κλικ στο **ερώτημα**.

    ![Ερώτημα επιλογής](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    Στην πύλη του Azure, κάντε κλικ στο **ερώτημα**.

    ![Επιλέξτε Προεπισκόπηση ερωτήματος](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  Νέες εργασίες έχουν ένα πρότυπο ερωτήματος για να σας βοηθήσουν να ξεκινήσετε. Το πρότυπο ερωτήματος εκτελεί ένα ερώτημα "διαβίβασης" που έργα όλα τα πεδία από εισαγωγής συμβάντα σε το αποτέλεσμα του.  

    - Εάν έχετε ορίσει τουλάχιστον μία εισόδου και εξόδου για την εργασία σας, μπορείτε να αντικαταστήσετε το σύμβολο κράτησης θέσης "[YourOutputAlias]" και "[YourInputAlias]" πεδία με τα ψευδώνυμα των εισόδου και εξόδου που θέλετε να χρησιμοποιήσετε πρώτα. Επιπλέον, να εξακολουθεί να συντάκτη και να ελέγξετε το ερώτημα στην πύλη κλασική Azure χωρίς τον ορισμό εισροές και εκροές στο έργο.
    - Εάν θέλετε να εκτελέσετε περισσότερες επεξεργασίας από ένα απλό διαβίβασης, μπορείτε να επεξεργαστείτε τον ορισμό του ερωτήματος. Για να ξεκινήσετε με το ερώτημα σύνταξη από κοινού, Ρίξτε μια ματιά ορισμένα κοινά ερωτήματος μοτίβα καταγράφονται [εδώ](stream-analytics-stream-analytics-query-patterns.md).  
  
    ![Δεδομένα ερωτήματος παραθύρου](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Για την επικύρωση δεδομένων ερωτήματος λειτουργεί: ##

Μπορείτε να ελέγξετε ότι το ερώτημά σας συμπεριφέρεται όπως αναμένεται, εκτελώντας στο πρόγραμμα περιήγησης επάνω από ένα ή περισσότερα τοπικό JSON αρχεία που περιέχει τα δεδομένα δοκιμής. Αυτό δεν θα ξεκινήσει η εργασία ή να έχετε οποιεσδήποτε συνέπειες χρέωσης.

> [AZURE.NOTE] Προς το παρόν δοκιμών ερωτημάτων στο πρόγραμμα περιήγησης δεν υποστηρίζεται στην πύλη του Azure.  

1.  Βεβαιωθείτε ότι δεν υπάρχουν σφάλματα στο ερώτημα (διαφορετικά το κουμπί δοκιμή θα απενεργοποιηθεί) και, στη συνέχεια, κάντε κλικ στο κουμπί έλεγχος.  

    ![Δεδομένα ερωτήματος δοκιμής](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  Θα σας ζητηθεί να καθορίσετε τα αρχεία για κάθε ένα από τα δεδομένα εισόδου στα οποία γίνεται αναφορά στο ερώτημα. Σε αυτό το παράδειγμα, το ερώτημα πρότυπο παραμένει ως-είναι, ώστε το παράθυρο διαλόγου γίνεται ερώτηση για είσοδο με το όνομα "yourinputalias".  

    ![Δοκιμή ερωτήματος δεδομένων](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  Αναζητήστε ένα αρχείο δοκιμής. Διάφορα αρχεία δείγματος είναι διαθέσιμες στην [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) και μπορείτε να ανακτήσετε δείγμα δεδομένων από τη δική σας εισόδου ροή δεδομένων μέσω της συνάρτησης δείγμα δεδομένων στην καρτέλα δεδομένα εισόδου.  

    ![Η είσοδος στο ερώτημα](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  Αφού κλείσετε το παράθυρο διαλόγου, το ερώτημα θα εκτελεστεί επάνω από τα δεδομένα δοκιμής και θα δείτε τα αποτελέσματα στο κάτω μέρος της σελίδας του ερωτήματος.  

    ![Σύνοψη ερωτήματος](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Λήψη Βοήθειας
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ ανάλυση ροή Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)
