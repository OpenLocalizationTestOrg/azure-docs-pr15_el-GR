<properties
   pageTitle="Ροή δεδομένων από ροή ανάλυσης στο χώρο αποθήκευσης δεδομένων λίμνης | Azure"
   description="Χρησιμοποιήστε ανάλυση Azure ροής για ροή δεδομένων στο χώρο αποθήκευσης λίμνης δεδομένων Azure"
   services="data-lake-store,stream-analytics" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Ροή δεδομένων από το Azure χώρο αποθήκευσης αντικειμένων Blob στο χώρο αποθήκευσης λίμνης δεδομένων με χρήση ανάλυσης ροή Azure

Σε αυτό το άρθρο θα μάθετε πώς μπορείτε να χρησιμοποιήσετε χώρου αποθήκευσης λίμνης Azure δεδομένων ως το αποτέλεσμα για μια εργασία Azure ροή ανάλυσης. Σε αυτό το άρθρο παρουσιάζει ένα απλό σενάριο που διαβάζει δεδομένα από μια Azure χώρο αποθήκευσης αντικειμένων blob (είσοδος) και γράφει τα δεδομένα στο χώρο αποθήκευσης δεδομένων λίμνης (Έξοδος).

>[AZURE.NOTE] Προς το παρόν, δημιουργία και ρύθμιση των παραμέτρων του χώρου αποθήκευσης δεδομένων λίμνης εξόδους για ανάλυση ροή υποστηρίζεται μόνο στην [Πύλη κλασική Azure](https://manage.windowsazure.com). Επομένως, ορισμένα τμήματα αυτού του προγράμματος εκμάθησης θα χρησιμοποιήσει την πύλη κλασική Azure.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

- **Ενεργοποίηση της συνδρομής σας Azure** για δημόσια προεπισκόπηση δεδομένων λίμνης χώρου αποθήκευσης. Ανατρέξτε στο θέμα [οδηγίες](data-lake-store-get-started-portal.md#signup).

- **Λογαριασμός azure χώρου αποθήκευσης**. Θα χρησιμοποιήσετε ένα κοντέινερ αντικειμένων blob από αυτόν το λογαριασμό για εισαγωγή δεδομένων για μια εργασία ροής ανάλυσης. Για αυτό το πρόγραμμα εκμάθησης, ας υποθέσουμε ότι δημιουργείτε ένα λογαριασμό χώρου αποθήκευσης που ονομάζεται **datalakestoreasa** και ένα κοντέινερ εντός του λογαριασμού που ονομάζεται **datalakestoreasacontainer**. Αφού δημιουργήσετε το κοντέινερ, αποστείλετε ένα δείγμα αρχείου δεδομένων σε αυτήν. Μπορείτε να λάβετε ένα δείγμα αρχείου δεδομένων από το [Azure δεδομένων λίμνης Git αποθετήριο](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Μπορείτε να χρησιμοποιήσετε διάφορα προγράμματα-πελάτες, όπως η [Εξερεύνηση χώρου αποθήκευσης Azure](http://storageexplorer.com/), για την αποστολή δεδομένων σε ένα κοντέινερ αντικειμένων blob.

    >[AZURE.NOTE] Εάν δημιουργήσετε το λογαριασμό από την πύλη του Azure, βεβαιωθείτε ότι δημιουργείτε με το μοντέλο **κλασική** ανάπτυξης. Αυτό εξασφαλίζει ότι είναι δυνατή η πρόσβαση στο λογαριασμό του χώρου αποθήκευσης από την πύλη κλασική Azure, καθώς αυτό είναι χρησιμοποιούμε για να δημιουργήσετε μια εργασία ροής ανάλυσης. Για οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης από την πύλη Azure χρησιμοποιώντας την ανάπτυξη κλασική, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού αποθήκευσης Azure](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Εναλλακτικά, θα μπορούσατε να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης από την πύλη κλασική Azure.

- **Λογαριασμός azure δεδομένων λίμνης Store**. Ακολουθήστε τις οδηγίες στο θέμα [Γρήγορα αποτελέσματα με με την πύλη Azure χώρου αποθήκευσης λίμνης δεδομένων Azure](data-lake-store-get-started-portal.md).  


## <a name="create-a-stream-analytics-job"></a>Δημιουργία μιας ροής εργασίας ανάλυσης

Μπορείτε να ξεκινήσετε με τη δημιουργία μιας εργασίας ροής ανάλυσης που περιλαμβάνει μια προέλευση εισόδου και ένα εξόδου προορισμού. Για αυτό το πρόγραμμα εκμάθησης, το αρχείο προέλευσης είναι ένα κοντέινερ αντικειμένων blob του Azure και ο προορισμός είναι χώρου αποθήκευσης λίμνης δεδομένων.

1. Είσοδος στην [πύλη του Azure κλασική](https://manage.windowsazure.com).

2. Από κάτω αριστερό μέρος της οθόνης, κάντε κλικ στην επιλογή **Δημιουργία**, **Υπηρεσίες δεδομένων**, **Ροή ανάλυση**, **Γρήγορη δημιουργία**. Παρέχουν τις τιμές, όπως φαίνεται παρακάτω και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία εργασίας ανάλυση ροής**.

    ![Δημιουργία μιας ροής εργασίας ανάλυσης] (./media/data-lake-store-stream-analytics/create.job.png "Δημιουργία μιας εργασίας ροής ανάλυσης")

## <a name="create-a-blob-input-for-the-job"></a>Δημιουργήστε ένα Blob εισόδου για το έργο

1. Ανοίξτε τη σελίδα για την εργασία ροής ανάλυση, κάντε κλικ στην καρτέλα **δεδομένα εισόδου** και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη είσοδο** για να ξεκινήσετε έναν οδηγό.

2. Στη σελίδα **Προσθήκη είσοδο με το έργο σας** , επιλέξτε **ροής δεδομένων**και, στη συνέχεια, κάντε κλικ στο βέλος προς τα εμπρός.

    ![Προσθήκη είσοδο με το έργο σας] (./media/data-lake-store-stream-analytics/create.input.1.png "Προσθήκη είσοδο με το έργο σας")

3. Στη σελίδα **Προσθήκη μια ροή δεδομένων με το έργο σας** , επιλέξτε **χώρο αποθήκευσης αντικειμένων Blob**και, στη συνέχεια, κάντε κλικ στο βέλος προς τα εμπρός.

    ![Προσθήκη μια ροή δεδομένων στο έργο] (./media/data-lake-store-stream-analytics/create.input.2.png "Προσθήκη μια ροή δεδομένων στο έργο")

4. Στη σελίδα **Ρυθμίσεις αποθήκευσης αντικειμένων Blob** , δώστε λεπτομέρειες για το χώρο αποθήκευσης αντικειμένων blob που θα χρησιμοποιήσετε ως προέλευση δεδομένων εισαγωγής.

    ![Παροχή του blob ρυθμίσεις χώρου αποθήκευσης] (./media/data-lake-store-stream-analytics/create.input.3.png "Παροχή του blob ρυθμίσεις χώρου αποθήκευσης")

    * **Enter ψευδωνύμου εισαγωγής**. Αυτό είναι ένα μοναδικό όνομα που δίνετε για την εργασία εισαγωγής.
    * **Επιλέξτε ένα λογαριασμό του χώρου αποθήκευσης**. Βεβαιωθείτε ότι ο λογαριασμός χώρου αποθήκευσης είναι στην ίδια περιοχή ως η εργασία ροής ανάλυση ή θα προκύψουν επιπλέον κόστος μετακίνηση δεδομένων μεταξύ περιοχών.
    * **Παροχή ένα κοντέινερ χώρου αποθήκευσης**. Μπορείτε να επιλέξετε για να δημιουργήσετε ένα νέο κοντέινερ ή επιλέξτε μια υπάρχουσα κοντέινερ.

    Κάντε κλικ στο βέλος προς τα εμπρός.

5. Στη σελίδα **Ρυθμίσεις σειριοποίησης** , ορίστε τη μορφή σειριοποίησης ως **CSV**, οριοθέτη ως **καρτέλα**, κωδικοποίηση ως **UTF8**, και, στη συνέχεια, κάντε κλικ στην επιλογή το σημάδι ελέγχου.

    ![Δώστε τις ρυθμίσεις σειριοποίησης] (./media/data-lake-store-stream-analytics/create.input.4.png "Δώστε τις ρυθμίσεις σειριοποίησης")

6. Όταν τελειώσετε με τον οδηγό, η εισαγωγή αντικειμένων blob θα προστεθούν κάτω από την καρτέλα **εισροές** και τη στήλη **διάγνωση** θα πρέπει να εμφανίζουν **OK**. Μπορείτε να ελέγξετε επίσης ρητά τη σύνδεση με την είσοδο, χρησιμοποιώντας το κουμπί **Δοκιμή σύνδεσης** στο κάτω μέρος.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Δημιουργία ενός χώρου αποθήκευσης λίμνης δεδομένων εξόδου για το έργο

1. Ανοίξτε τη σελίδα για την εργασία ροής ανάλυση, κάντε κλικ στην καρτέλα **εξόδους** και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη το αποτέλεσμα** για να εμφανίσετε έναν οδηγό.

2. Στη σελίδα **Προσθήκη το αποτέλεσμα με το έργο σας** , επιλέξτε **Χώρου αποθήκευσης λίμνης δεδομένων**και, στη συνέχεια, κάντε κλικ στο βέλος προς τα εμπρός.

    ![Προσθήκη το αποτέλεσμα με το έργο σας] (./media/data-lake-store-stream-analytics/create.output.1.png "Προσθήκη το αποτέλεσμα με το έργο σας")

3. Στη σελίδα **εξουσιοδότηση σύνδεσης** , εάν έχετε ήδη δημιουργήσει ένα λογαριασμό του χώρου αποθήκευσης λίμνης δεδομένων, κάντε κλικ στην επιλογή **Εγκρίνετε τώρα**. Διαφορετικά, κάντε κλικ στην επιλογή **εγγραφείτε τώρα** για να δημιουργήσετε ένα νέο λογαριασμό. Σε αυτό το πρόγραμμα εκμάθησης, ας υποθέσουμε ότι έχετε ήδη ένα λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων (όπως που αναφέρεται στο την προϋπόθεση). Που θα αυτόματα επιτρέπεται χρησιμοποιώντας τα διαπιστευτήρια με τα οποία έχετε εισέλθει στο Azure κλασική πύλη.

    ![Εγκρίνετε χώρου αποθήκευσης δεδομένων λίμνης] (./media/data-lake-store-stream-analytics/create.output.2.png "Εγκρίνετε χώρου αποθήκευσης δεδομένων λίμνης")

4. Στη σελίδα **Ρυθμίσεις αποθήκευσης λίμνης δεδομένων** , εισαγάγετε τις πληροφορίες, όπως φαίνεται στην παρακάτω την εικόνα της οθόνης.

    ![Ρυθμίσεις χώρου αποθήκευσης λίμνης Καθορισμός δεδομένων] (./media/data-lake-store-stream-analytics/create.output.3.png "Ρυθμίσεις χώρου αποθήκευσης λίμνης Καθορισμός δεδομένων")

    * **Enter ένα ψευδώνυμο εξόδου**. Αυτό είναι ένα μοναδικό όνομα που δίνετε για το αποτέλεσμα του έργου.
    * **Καθορισμός ενός λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων**. Θα πρέπει να έχετε ήδη δημιουργήσει αυτή, όπως αναφέρεται στην την προϋπόθεση.
    * **Καθορίστε ένα μοτίβο πρόθεμα διαδρομή**. Αυτό είναι απαραίτητο για να προσδιορίσετε τα αρχεία εξόδου που εγγράφονται στο χώρο αποθήκευσης λίμνης δεδομένων από την εργασία ροής ανάλυσης. Επειδή οι τίτλοι των εξόδους από η εργασία είναι σε μορφή GUID, όπως ένα πρόθεμα θα σας βοηθήσει τον προσδιορισμό του χειρόγραφων εξόδου. Εάν θέλετε να συμπεριλάβετε μια σήμανση ημερομηνίας και ώρας ως μέρος του προθέματος φροντίστε να συμπεριλάβετε `{date}/{time}` του μοτίβου πρόθεμα. Εάν συμπεριλάβετε αυτήν, τα πεδία ημερομηνίας και της **Μορφής ** **Ώρας** είναι ενεργοποιημένες και μπορείτε να επιλέξετε τη μορφή της επιλογής.

    Κάντε κλικ στο βέλος προς τα εμπρός.

5. Στη σελίδα **Ρυθμίσεις σειριοποίησης** , ορίστε τη μορφή σειριοποίησης ως **CSV**, οριοθέτη ως **καρτέλα**, κωδικοποίηση ως **UTF8**, και, στη συνέχεια, κάντε κλικ στο σημάδι ελέγχου.

    ![Καθορίστε τη μορφή εξόδου] (./media/data-lake-store-stream-analytics/create.output.4.png "Καθορίστε τη μορφή εξόδου")

6. Όταν τελειώσετε με τον οδηγό, το αποτέλεσμα του χώρου αποθήκευσης δεδομένων λίμνης θα προστεθεί στην καρτέλα **εξόδους** και τη στήλη **διάγνωση** θα πρέπει να εμφανίζουν το **κουμπί OK**. Μπορείτε να ελέγξετε επίσης ρητά τη σύνδεση με το αποτέλεσμα, χρησιμοποιώντας το κουμπί **Δοκιμή σύνδεσης** στο κάτω μέρος.

## <a name="run-the-stream-analytics-job"></a>Εκτέλεση της ανάλυσης ροής εργασίας

Για να εκτελέσετε μια εργασία ροής ανάλυσης, πρέπει να εκτελέσετε ένα ερώτημα από την καρτέλα ερώτημα. Για αυτό το πρόγραμμα εκμάθησης, μπορείτε να εκτελέσετε το ερώτημα δείγμα, αντικαθιστώντας τα σύμβολα κράτησης θέσης με την εργασία εισόδου και εξόδου ψευδώνυμα, όπως φαίνεται στην παρακάτω την εικόνα της οθόνης.

![Εκτέλεση ερωτήματος] (./media/data-lake-store-stream-analytics/run.query.png "Εκτέλεση ερωτήματος")

Κάντε κλικ στην επιλογή **Αποθήκευση** από το κάτω μέρος της οθόνης και, στη συνέχεια, κάντε κλικ στο κουμπί **Έναρξη**. Από το παράθυρο διαλόγου, επιλέξτε **Προσαρμοσμένη ώρα**και, στη συνέχεια, επιλέξτε μια ημερομηνία από το παρελθόν, όπως **1/1/2016**. Κάντε κλικ στο σημάδι ελέγχου για να ξεκινήσει η εργασία. Αυτό μπορεί να διαρκέσει μερικά λεπτά για να ξεκινήσει η εργασία.

![Ορισμός χρόνου εργασίας] (./media/data-lake-store-stream-analytics/run.query.2.png "Ορισμός χρόνου εργασίας")

Αφού ξεκινήσει η εργασία, κάντε κλικ στην καρτέλα **οθόνη** για να δείτε πώς έγινε επεξεργασία των δεδομένων.

![Παρακολούθηση εργασίας] (./media/data-lake-store-stream-analytics/run.query.3.png "Παρακολούθηση εργασίας")

Τέλος, μπορείτε να χρησιμοποιήσετε την [Πύλη του Azure](https://portal.azure.com) για να ανοίξετε το λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων και να επαληθεύσετε εάν τα δεδομένα έχει συνταχθεί με επιτυχία στο λογαριασμό.

![Επαλήθευση εξόδου] (./media/data-lake-store-stream-analytics/run.query.4.png "Επαλήθευση εξόδου")

Στο παράθυρο Εξερεύνηση δεδομένων, παρατηρήστε ότι το αποτέλεσμα είναι γραμμένο σε ένα φάκελο, όπως καθορίζονται στο χώρο αποθήκευσης δεδομένων λίμνης ρυθμίσεις εξόδου (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Δείτε επίσης

* [Δημιουργήστε ένα σύμπλεγμα HDInsight για να χρησιμοποιήσετε το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)