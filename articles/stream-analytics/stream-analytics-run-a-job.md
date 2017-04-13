<properties 
    pageTitle="Πώς να ξεκινήσετε τη ροή εργασιών στην ανάλυση ροή | Microsoft Azure" 
    description="Τον τρόπο εκτέλεσης μιας ροής εργασίας στην ανάλυση ροή Azure | εκμάθηση τμήμα διαδρομής."
    keywords="η ροή εργασιών"
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

# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Τρόπος εκτέλεσης μιας ροής εργασίας σε ανάλυση ροή Azure

Όταν μια εργασία εισαγωγής, ερωτήματος και εξόδου όλα έχουν καθοριστεί που μπορεί να ξεκινήσει η εργασία ροής ανάλυσης.

Για να ξεκινήσετε την εργασία σας:

1.  Στην πύλη του Azure κλασική, από τον πίνακα εργαλείων εργασία, κάντε κλικ στο κουμπί " **Έναρξη** " στο κάτω μέρος της σελίδας.

    ![Έναρξη εργασίας κουμπί](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    Στην πύλη του Azure, κάντε κλικ στην επιλογή **Έναρξη** στο επάνω μέρος της σελίδας του έργου.

    ![Azure πύλης εργασία Έναρξη κουμπί](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  Καθορίστε μια τιμή **Έναρξης εξόδου** για να καθορίσετε πότε αυτή η εργασία θα ξεκινήσει παράγει δεδομένα εξόδου. Η προεπιλεγμένη ρύθμιση για τις εργασίες που δεν έχουν ξεκινήσει προηγουμένως είναι **Ώρα έναρξης του έργου**, γεγονός που σημαίνει ότι η εργασία θα ξεκινήσει αμέσως επεξεργασίας δεδομένων. Μπορείτε επίσης να καθορίσετε μια **προσαρμοσμένη** ώρα στο παρελθόν (για κατανάλωση παλαιότερα στοιχεία) ή το μέλλον (για να καθυστερήσετε επεξεργασίας μέχρι μέλλον). Για τις περιπτώσεις όταν μια εργασία έχει ήδη ξεκινήσει ή διακοπεί, η επιλογή **Τελευταία φορά διακοπεί** είναι διαθέσιμη για να συνεχίσετε την εργασία από την τελευταία φορά εξόδου και να αποφύγετε την απώλεια δεδομένων.  

    ![Έναρξη ροής εργασίας χρόνου](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure πύλης Έναρξη ροής εργασίας χρόνου](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  Επιβεβαιώστε την επιλογή σας. Την κατάσταση της εργασίας θα αλλάξει σε *Έναρξη* και θα λίγο Μετακίνηση *εκτελείται* αφού έχει ξεκινήσει η εργασία. Μπορείτε να παρακολουθείτε την πρόοδο της λειτουργίας **Ξεκινήστε** από την **Ενότητα "Ειδοποίηση"**:

    ![η ροή εργασίας προόδου](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![Azure πύλη ροή της προόδου εργασίας](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Λήψη Βοήθειας
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ ανάλυση ροή Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)
