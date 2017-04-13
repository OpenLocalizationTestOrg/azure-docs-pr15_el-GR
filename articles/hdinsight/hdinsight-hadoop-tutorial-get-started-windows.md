<properties
   pageTitle="Πρόγραμμα εκμάθησης Hadoop: γρήγορα αποτελέσματα με το Hadoop στα Windows | Microsoft Azure"
   description="Γρήγορα αποτελέσματα με Hadoop σε HDInsight. Μάθετε πώς μπορείτε να δημιουργήσετε συμπλεγμάτων Hadoop στα Windows, να εκτελέσετε ένα ερώτημα Hive δεδομένα και να αναλύετε εξόδου στο Excel."
   keywords="πρόγραμμα εκμάθησης hadoop, hadoop στα windows, σύμπλεγμα hadoop, μάθετε hadoop, ομάδα ερωτήματος"
   services="hdinsight"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="03/07/2016"
   ms.author="nitinme"/>


# <a name="hadoop-tutorial-get-started-using-hadoop-in-hdinsight-on-windows"></a>Πρόγραμμα εκμάθησης Hadoop: γρήγορα αποτελέσματα με το Hadoop στο HDInsight στα Windows

> [AZURE.SELECTOR]
- [Βάσει Linux](../hdinsight-hadoop-linux-tutorial-get-started.md)
- [Βασίζεται σε Windows](../hdinsight-hadoop-tutorial-get-started-windows.md)

Για να σας βοηθήσει να μάθετε Hadoop στα Windows και να ξεκινήσετε να χρησιμοποιείτε το HDInsight, αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να εκτελέσετε ένα ερώτημα Hive σε μη δομημένα δεδομένα σε ένα σύμπλεγμα Hadoop και, στη συνέχεια, να αναλύσετε τα αποτελέσματα στο Microsoft Excel.

>[AZURE.NOTE] Οι πληροφορίες σε αυτό το έγγραφο είναι συγκεκριμένη για συμπλεγμάτων HDInsight που βασίζεται στα Windows. Για πληροφορίες σχετικά με βάση Linux συμπλεγμάτων, ανατρέξτε στο θέμα [Hadoop πρόγραμμα εκμάθησης: γρήγορα αποτελέσματα με το Linux βάσει Hadoop στο HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

Ας υποθέσουμε ότι έχετε ένα μεγάλο σύνολο δεδομένων μη δομημένα και θέλετε να εκτελέσετε ένα ερώτημα της ομάδας, για να εξαγάγετε ορισμένες σημαντικές πληροφορίες. Που είναι ακριβώς τι πρόκειται να κάνετε σε αυτό το πρόγραμμα εκμάθησης. Δείτε τώρα πώς να επιτύχετε το εξής:

   !["Hadoop πρόγραμμα εκμάθησης: Δημιουργία λογαριασμού; Δημιουργήστε ένα σύμπλεγμα Hadoop; υποβολή ενός ερωτήματος ομάδα; ανάλυση των δεδομένων στο Excel.][image-hdi-getstarted-flow]

Παρακολουθήστε ένα βίντεο επίδειξης αυτού του προγράμματος εκμάθησης για να μάθετε Hadoop στην HDInsight:

![Βίντεο από μια πρώτη Hadoop το πρόγραμμα εκμάθησης: υποβολή ενός ερωτήματος ομάδας σε ένα σύμπλεγμα Hadoop, και ανάλυση αποτελεσμάτων στο Excel.][img-hdi-getstarted-video]

**[Παρακολουθήστε το πρόγραμμα εκμάθησης Hadoop για HDInsight στο YouTube](https://www.youtube.com/watch?v=Y4aNjnoeaHA&list=PLDrz-Fkcb9WWdY-Yp6D4fTC1ll_3lU-QS)**

Σε συνδυασμό με τη γενική διαθεσιμότητα του Azure HDInsight, η Microsoft παρέχει επίσης HDInsight προσομοίωσης για Azure, παλαιότερα γνωστό ως *Προεπισκόπηση για τους προγραμματιστές Microsoft HDInsight*. Το προσομοίωσης ως προορισμό σενάρια προγραμματιστής και υποστηρίζει μόνο μίας κόμβου αναπτύξεις. Για πληροφορίες σχετικά με τη χρήση προσομοίωσης HDInsight, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το HDInsight προσομοίωσης][hdinsight-emulator].

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης για Hadoop στα Windows, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **A σταθμούς εργασίας υπολογιστή** με το Office 2013 Professional Plus, Office 365 Pro Plus, αυτόνομη έκδοση του Excel 2013 ή Office 2010 Professional Plus.

### <a name="access-control-requirements"></a>Απαιτήσεις για στοιχείο ελέγχου πρόσβασης

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-hadoop-clusters"></a>Δημιουργία Hadoop συμπλεγμάτων

Όταν δημιουργείτε ένα σύμπλεγμα, μπορείτε να δημιουργήσετε πόρους Azure υπολογισμού που περιέχουν Hadoop και σχετικές εφαρμογές. Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα σύμπλεγμα έκδοση 3.2 HDInsight. Μπορείτε επίσης να δημιουργήσετε Hadoop συμπλεγμάτων για τις άλλες εκδόσεις. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία HDInsight συμπλεγμάτων χρησιμοποιώντας προσαρμοσμένες επιλογές][hdinsight-provision]. Για πληροφορίες σχετικά με τις εκδόσεις HDInsight και τους SLA, ανατρέξτε στο θέμα [Διαχείριση εκδόσεων στοιχείου HDInsight](hdinsight-component-versioning.md).


**Για να δημιουργήσετε ένα σύμπλεγμα Hadoop**

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com/).
2. Κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ**, κάντε κλικ στην επιλογή **Ανάλυση δεδομένων**και, στη συνέχεια, κάντε κλικ στην επιλογή **HDInsight**. Η πύλη ανοίγει ένα **Νέο σύμπλεγμα HDInsight** blade.

    ![Δημιουργία ενός νέου συμπλέγματος στην πύλη του Azure] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.1.png "Δημιουργία ενός νέου συμπλέγματος στην πύλη του Azure")

3. Πληκτρολογήστε ή επιλέξτε τα εξής:

    ![Πληκτρολογήστε το όνομα συμπλέγματος και πληκτρολογήστε] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.2.png "Πληκτρολογήστε το όνομα συμπλέγματος και πληκτρολογήστε")
    
  	|Όνομα πεδίου| Τιμή|
  	|----------|------|
  	|Όνομα συμπλέγματος| Ένα μοναδικό όνομα για τον προσδιορισμό του συμπλέγματος|
  	|Τύπος συμπλέγματος| Επιλέξτε **Hadoop** για αυτό το πρόγραμμα εκμάθησης. |
  	|Σύμπλεγμα λειτουργικό σύστημα| Επιλέξτε **Windows Server 2012 R2 κέντρο δεδομένων** για αυτό το πρόγραμμα εκμάθησης.|
  	|Έκδοση HDInsight| Επιλέξτε την πιο πρόσφατη έκδοση για αυτό το πρόγραμμα εκμάθησης.|
  	|Συνδρομή| Επιλέξτε τη συνδρομή Azure που θα χρησιμοποιηθεί για το σύμπλεγμα.|
  	|Ομάδα πόρων | Επιλέξτε μια υπάρχουσα ομάδα Azure πόρων ή δημιουργήστε μια νέα ομάδα πόρων. Ένα βασικό σύμπλεγμα HDInsight περιέχει ένα σύμπλεγμα και του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης.  Μπορείτε να ομαδοποιήσετε τα δύο σε μια ομάδα πόρων για εύκολη διαχείριση.|
  	|Τα διαπιστευτήρια| Πληκτρολογήστε το όνομα χρήστη σύνδεσης σύμπλεγμα και τον κωδικό πρόσβασης. Σύμπλεγμα με βάση τα Windows μπορούν να έχουν 2 λογαριασμούς χρήστη.  Το σύμπλεγμα χρήστη (ή χρήστη HTTP) χρησιμοποιείται για τη διαχείριση του συμπλέγματος και υποβολή εργασιών.  Προαιρετικά, μπορείτε να δημιουργήσετε μια σύνδεση απομακρυσμένης επιφάνειας εργασίας (RDP) λογαριασμό χρήστη σε απομακρυσμένο συνδεθείτε με το σύμπλεγμα. Εάν επιλέξετε να ενεργοποιήσετε απομακρυσμένης επιφάνειας εργασίας, θα δημιουργήσετε το λογαριασμό χρήστη RDP.|
  	|Αρχείο προέλευσης δεδομένων| Κάντε κλικ στην επιλογή Δημιουργία νέου, για να δημιουργήσετε μια νέα προεπιλεγμένου λογαριασμού Azure χώρου αποθήκευσης. Χρησιμοποιήστε το όνομα του συμπλέγματος ως το προεπιλεγμένο όνομα κοντέινερ. Κάθε σύμπλεγμα HDinsight έχει ένα προεπιλεγμένο κοντέινερ αντικειμένων Blob μια εκχώρηση Azure χώρου αποθήκευσης.  Τη θέση του τον προεπιλεγμένο λογαριασμό Azure αποθήκευσης Καθορίζει τη θέση του συμπλέγματος HDInsight.|
  	|Τις τιμές βαθμίδες κόμβου| Χρησιμοποιήστε το 1 ή 2 κόμβους εργαζόμενου με την προεπιλεγμένη εργαζόμενου κόμβο και κεφαλή Σημείωση Τιμολόγηση επιπέδων για αυτό το πρόγραμμα εκμάθησης.|
  	|Προαιρετική ρύθμιση παραμέτρων| Παράλειψη αυτού του τμήματος.|

9. Στην blade το **Νέο σύμπλεγμα HDInsight** , βεβαιωθείτε ότι είναι επιλεγμένο **Pin για να Startboard** και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**. Αυτό θα δημιουργήσετε το σύμπλεγμα και να προσθέσετε ένα πλακίδιο για αυτήν την Startboard της πύλης Azure. Το εικονίδιο υποδεικνύει ότι το σύμπλεγμα είναι να δημιουργήσετε και θα αλλάξει για να εμφανίσετε το εικονίδιο HDInsight μόλις ολοκληρωθεί η δημιουργία.

  	| Κατά τη δημιουργία | Δημιουργία ολοκληρώθηκε |
  	| ------------------ | --------------------- |
  	| ![Δημιουργία ένδειξη σε startboard](./media/hdinsight-hadoop-tutorial-get-started-windows/provisioning.png) | ![Δημιουργήθηκε πλακίδιο συμπλέγματος](./media/hdinsight-hadoop-tutorial-get-started-windows/provisioned.png) |

    > [AZURE.NOTE] Θα χρειαστεί κάποιος χρόνος για το σύμπλεγμα να δημιουργηθεί, συνήθως είναι περίπου 15 λεπτά. Χρησιμοποιήστε το πλακίδιο στην το Startboard ή την καταχώρηση **ειδοποιήσεις** στην αριστερή πλευρά της σελίδας για να ελέγξετε τη διαδικασία δημιουργίας.

10. Μόλις ολοκληρωθεί η δημιουργία, κάντε κλικ στο πλακίδιο για το σύμπλεγμα από το Startboard να εκκινήσετε το σύμπλεγμα blade.


## <a name="run-a-hive-query-from-the-portal"></a>Εκτέλεση ενός ερωτήματος ομάδας από την πύλη
Τώρα που έχετε δημιουργήσει ένα σύμπλεγμα HDInsight, το επόμενο βήμα είναι να εκτελέσετε μια εργασία Hive ερώτημα για ένα δείγμα πίνακα ομάδα. Θα χρησιμοποιήσουμε *hivesampletable*, που παρέχεται με το HDInsight συμπλεγμάτων. Ο πίνακας περιέχει δεδομένα σχετικά με την κινητή συσκευή κατασκευαστές, πλατφόρμες και μοντέλα. Ομάδα ερωτήματος σε αυτόν τον πίνακα ανακτά δεδομένα για κινητές συσκευές με ένα συγκεκριμένο κατασκευαστή.

> [AZURE.NOTE] Εργαλεία HDInsight για το Visual Studio συνοδεύεται από το Azure SDK για .NET έκδοση 2.5 ή νεότερη έκδοση. Χρησιμοποιώντας τα εργαλεία στο Visual Studio, μπορείτε να συνδεθείτε με σύμπλεγμα HDInsight, δημιουργία πινάκων Hive και εκτέλεση ερωτημάτων Hive. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το HDInsight Hadoop εργαλεία για το Visual Studio][1].

**Για να εκτελέσετε μια εργασία ομάδας από τον πίνακα εργαλείων συμπλέγματος**

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com/).
2. Κάντε κλικ στην επιλογή **ΑΝΑΖΉΤΗΣΗ ΌΛΩΝ** και, στη συνέχεια, κάντε κλικ στην επιλογή **HDInsight συμπλεγμάτων** για να δείτε μια λίστα των συμπλεγμάτων, συμπεριλαμβανομένου του συμπλέγματος που μόλις δημιουργήσατε στην προηγούμενη ενότητα.
3. Κάντε κλικ στο όνομα του συμπλέγματος που θέλετε να χρησιμοποιήσετε για την εκτέλεση της ομάδας εργασίας και, στη συνέχεια, κάντε κλικ στην επιλογή **Πίνακας εργαλείων** στο επάνω μέρος του blade.
4. Ανοίγει μια ιστοσελίδα σε μια καρτέλα διαφορετικό πρόγραμμα περιήγησης. Πληκτρολογήστε το λογαριασμό χρήστη Hadoop και τον κωδικό πρόσβασης. Το προεπιλεγμένο όνομα χρήστη είναι **διαχειριστής**; ο κωδικός πρόσβασης είναι τι έχετε εισαγάγει κατά τη δημιουργία του συμπλέγματος.
5. Από τον πίνακα εργαλείων, κάντε κλικ στην καρτέλα **Hive επεξεργασίας** . Ανοίγει η σελίδα web παρακάτω.

    ![Καρτέλα προγράμματος επεξεργασίας Hive στον πίνακα εργαλείων σύμπλεγμα HDInsight.][img-hdi-dashboard]

    Υπάρχουν πολλές καρτέλες στο επάνω μέρος της σελίδας. Στην προεπιλεγμένη καρτέλα είναι **Hive επεξεργασίας**και τις άλλες καρτέλες **Ιστορικού εργασίας** και το **Πρόγραμμα περιήγησης του αρχείου**. Χρησιμοποιώντας τον πίνακα εργαλείων, μπορείτε να υποβολή ερωτημάτων Hive, ελέγξτε τα αρχεία καταγραφής Hadoop εργασία και αναζήτηση των αρχείων στο χώρο αποθήκευσης.

    > [AZURE.NOTE] Σημειώστε ότι η διεύθυνση URL της ιστοσελίδας είναι * &lt;ClusterName&gt;. azurehdinsight.net*. Επομένως, αντί για το άνοιγμα του πίνακα εργαλείων από την πύλη, μπορείτε να ανοίξετε τον πίνακα εργαλείων από ένα πρόγραμμα περιήγησης web, χρησιμοποιώντας τη διεύθυνση URL.

6. Στην καρτέλα **Hive επεξεργασίας** , **Όνομα ερωτήματος**, πληκτρολογήστε **HTC20**.  Το όνομα του ερωτήματος είναι ο τίτλος εργασίας. Στο παράθυρο ερωτήματος, εισαγάγετε το ερώτημα ομάδας, όπως φαίνεται στην εικόνα:

    ![Ομάδα ερώτημα που καταχωρείται στο παράθυρο ερωτήματος της επεξεργασίας της ομάδας.][img-hdi-dashboard-query-select]

4. Κάντε κλικ στην επιλογή **Υποβολή**. Χρειάζονται μερικά λεπτά για να επιστρέψετε τα αποτελέσματα. Στην οθόνη ανανεώνει κάθε 30 δευτερόλεπτα. Μπορείτε επίσης να επιλέξετε " **Ανανέωση** " για την ανανέωση της οθόνης.

    ![Αποτελέσματα από ένα ερώτημα Hive σε εμφανίζεται στο κάτω μέρος του πίνακα εργαλείων σύμπλεγμα.][img-hdi-dashboard-query-select-result]

5. Μετά την κατάσταση δείχνει ότι η εργασία έχει ολοκληρωθεί, κάντε κλικ στο όνομα του ερωτήματος στην οθόνη για να δείτε το αποτέλεσμα. Σημειώστε **Εργασία Έναρξη ώρα (UTC)**. Θα το χρειαστείτε αργότερα.

    ![Έναρξη χρόνος εργασίας που εμφανίζεται στην καρτέλα Ιστορικό εργασίας του πίνακα εργαλείων σύμπλεγμα HDInsight.][img-hdi-dashboard-query-select-result-output]

    Η σελίδα εμφανίζει επίσης το **Αποτέλεσμα του έργου** και το **Αρχείο καταγραφής εργασία**. Έχετε επίσης την επιλογή για να κάνετε λήψη του αρχείου εξόδου (\_stdout) και το αρχείο καταγραφής \(_stderr).


**Για να μεταβείτε στο αρχείο εξόδου**

1. Στον πίνακα εργαλείων του συμπλέγματος, κάντε κλικ στην επιλογή **Αρχείο προγράμματος περιήγησης**.
2. Κάντε κλικ στο όνομα του λογαριασμού σας χώρου αποθήκευσης, κάντε κλικ στο όνομα του κοντέινερ (το οποίο είναι το ίδιο με το όνομα συμπλέγματος) και, στη συνέχεια, κάντε κλικ στην επιλογή **χρήστη**.
3. Κάντε κλικ στην επιλογή **Διαχείριση** και, στη συνέχεια, κάντε κλικ στο GUID που έχει ώρας της τελευταίας τροποποίησης (λίγο αφού η εργασία ξεκινά ώρα σημειώσατε προηγουμένως). Αντιγράψτε αυτό το GUID. Θα το χρειαστείτε στην επόμενη ενότητα.


    ![Το ερώτημα Hive εξόδου αρχείου GUID που εμφανίζεται στην καρτέλα αρχείο προγράμματος περιήγησης.][img-hdi-dashboard-query-browse-output]


##<a name="connect-to-microsoft-business-intelligence-tools-for-excel"></a>Σύνδεση με εργαλεία επιχειρηματικής ευφυΐας της Microsoft για το Excel

Μπορείτε να χρησιμοποιήσετε το Power Query προσθέτου για το Microsoft Excel για να εισαγάγετε το αποτέλεσμα του έργου από το HDInsight στο Excel, όπου μπορούν να χρησιμοποιηθούν εργαλεία επιχειρηματικής ευφυΐας της Microsoft για περαιτέρω ανάλυση των αποτελεσμάτων.

Πρέπει να έχετε Excel 2013 ή 2010 εγκατεστημένο για να ολοκληρώσετε αυτό το τμήμα του προγράμματος εκμάθησης.

**Για να κάνετε λήψη του Microsoft Power Query για Excel**

- Κάντε λήψη του Microsoft Power Query για το Microsoft Excel από το [Κέντρο λήψης της Microsoft](http://www.microsoft.com/download/details.aspx?id=39379) και εγκαταστήστε το.

**Για να εισαγάγετε δεδομένα HDInsight**

1. Ανοίξτε το Excel και δημιουργήστε ένα νέο βιβλίο εργασίας.
3. Κάντε κλικ στο μενού του **Power Query** , κάντε κλικ στην επιλογή **Από άλλες προελεύσεις**και, στη συνέχεια, κάντε κλικ στην επιλογή **Από το Azure HDInsight**.

    ![Εισαγωγή PowerQuery Excel Άνοιγμα μενού για Azure HDInsight.][image-hdi-gettingstarted-powerquery-importdata]

3. Πληκτρολογήστε το **Όνομα του λογαριασμού** του λογαριασμού χώρο αποθήκευσης Blob του Azure που είναι συσχετισμένη με το σύμπλεγμά σας και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**. (Αυτό είναι το λογαριασμό χώρου αποθήκευσης που δημιουργήσατε νωρίτερα σε το πρόγραμμα εκμάθησης.)
4. Πληκτρολογήστε το **Κλειδί λογαριασμού** για το χώρο αποθήκευσης Blob του Azure λογαριασμό και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.
5. Στο δεξιό παράθυρο, κάντε διπλό κλικ στο όνομα blob. Από προεπιλογή το όνομα blob είναι το ίδιο με το όνομα του συμπλέγματος.

6. Εντοπίστε **stdout** στη στήλη **όνομα** . Βεβαιωθείτε ότι το GUID στην αντίστοιχη στήλη **Διαδρομή φακέλου** συμφωνεί με το GUID που αντιγράψατε προηγουμένως. Συμφωνία προτείνει ότι τα δεδομένα εξόδου που αντιστοιχεί στην εργασία που υποβάλλεται. Κάντε κλικ στην επιλογή **δυαδικό** στην αριστερή στήλη του **stdout**.

    ![Μπορείτε να βρείτε τα δεδομένα εξόδου από GUID στη λίστα περιεχομένου.][image-hdi-gettingstarted-powerquery-importdata2]

9. Κάντε κλικ στο κουμπί **Κλείσιμο και φόρτωση** στην επάνω αριστερή γωνία για να εισαγάγετε την εργασία Hive εξόδου στο Excel.

##<a name="run-samples"></a>Εκτέλεση δείγματα

Σύμπλεγμα HDInsight παρέχει μια κονσόλα ερωτήματος που περιλαμβάνει μια συλλογή γρήγορα αποτελέσματα για να εκτελέσετε δείγματα απευθείας από την πύλη. Μπορείτε να χρησιμοποιήσετε τα δείγματα για να μάθετε πώς μπορείτε να εργαστείτε με HDInsight, ενέργειες μέσω ορισμένα βασικά σενάρια. Αυτά τα δείγματα περιλαμβάνει όλα τα απαιτούμενα στοιχεία, όπως τα δεδομένα για την ανάλυση και τα ερωτήματα για να εκτελέσετε στα δεδομένα σας. Για να μάθετε περισσότερα σχετικά με τα δείγματα στη συλλογή "Γρήγορα αποτελέσματα", ανατρέξτε στο θέμα [Μάθετε Hadoop στο HDInsight χρησιμοποιώντας τη συλλογή HDInsight γρήγορα αποτελέσματα](hdinsight-learn-hadoop-use-sample-gallery.md).

**Για να εκτελέσετε το δείγμα**

1. Από την πύλη Azure startboard, κάντε κλικ στο πλακίδιο για το σύμπλεγμα που μόλις δημιουργήσατε.
 
2. Στην το νέο blade σύμπλεγμα, κάντε κλικ στην επιλογή **πίνακα εργαλείων**. Όταν σας ζητηθεί, πληκτρολογήστε το όνομα χρήστη του διαχειριστή και τον κωδικό πρόσβασης για το σύμπλεγμα.

    ![Εκκίνηση σύμπλεγμα πίνακα εργαλείων] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.Cluster.Dashboard.png "Εκκίνηση σύμπλεγμα πίνακα εργαλείων")
 
3. Από την ιστοσελίδα που ανοίγει, κάντε κλικ στην καρτέλα **Συλλογή γρήγορα αποτελέσματα** και, στη συνέχεια, στην κατηγορία **λύσεις με δείγμα δεδομένων** , κάντε κλικ στην επιλογή το δείγμα που θέλετε να εκτελέσετε. Ακολουθήστε τις οδηγίες στην ιστοσελίδα για να ολοκληρώσετε το δείγμα. Ο παρακάτω πίνακας παραθέτει μερικά δείγματα και παρέχει περισσότερες πληροφορίες σχετικά με κάθε δείγμα τι κάνει.

Δείγμα | Τι κάνει αυτό;
------ | ---------------
[Ανάλυση δεδομένων αισθητήρα][hdinsight-sensor-data-sample] | Μάθετε πώς μπορείτε να χρησιμοποιήσετε HDInsight για να επεξεργαστείτε τα παλαιότερα στοιχεία που παράγεται από συστήματα θέρμανσης, εξαερισμού και κλιματισμού (HVAC) συστήματα για τον προσδιορισμό συστήματα που δεν έχουν τη δυνατότητα να αξιόπιστα διατηρεί τη θερμοκρασία σύνολο.
[Τοποθεσία Web του αρχείου καταγραφής ανάλυσης][hdinsight-weblogs-sample] | Μάθετε πώς να χρησιμοποιείτε το HDInsight για να αναλύσετε αρχεία καταγραφής τοποθεσίας Web για να λάβετε πληροφορίες για τη συχνότητα επισκέψεων στην τοποθεσία Web σε μια ημέρα από εξωτερικές τοποθεσίες Web και μια σύνοψη των σφαλμάτων τοποθεσίας Web που αντιμετωπίζουν οι χρήστες.
[Ανάλυση τάσεων Twitter](hdinsight-analyze-twitter-data.md) | Μάθετε πώς μπορείτε να χρησιμοποιήσετε το HDInsight για να αναλύσετε τάσεις στα Twitter.

##<a name="delete-the-cluster"></a>Διαγραφή του συμπλέγματος

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το πρόγραμμα εκμάθησης Hadoop, μάθατε πώς μπορείτε να δημιουργήσετε ένα σύμπλεγμα Hadoop των Windows σε HDInsight, εκτέλεση ενός ερωτήματος Hive σε δεδομένα και να εισάγετε τα αποτελέσματα στο Excel, όπου είναι δυνατή η περαιτέρω επεξεργασία και γραφικά εμφανίζονται με εργαλεία επιχειρηματικής ευφυΐας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα τα παρακάτω προγράμματα εκμάθησης:

- [Γρήγορα αποτελέσματα με το HDInsight Hadoop εργαλεία για το Visual Studio][1]
- [Γρήγορα αποτελέσματα με το HDInsight προσομοίωσης][hdinsight-emulator]
- [Χρήση του χώρου αποθήκευσης αντικειμένων Blob του Azure με HDInsight][hdinsight-storage]
- [Διαχείριση HDInsight χρησιμοποιώντας PowerShell][hdinsight-admin-powershell]
- [Αποστολή δεδομένων σε HDInsight][hdinsight-upload-data]
- [Χρήση MapReduce με HDInsight][hdinsight-use-mapreduce]
- [Χρήση της ομάδας με το HDInsight][hdinsight-use-hive]
- [Χρήση γουρούνι με HDInsight][hdinsight-use-pig]
- [Χρήση Oozie με HDInsight][hdinsight-use-oozie]
- [Ανάπτυξη προγραμμάτων Java MapReduce για HDInsight][hdinsight-develop-mapreduce]


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-versions]: hdinsight-component-versioning.md


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-emulator]: hdinsight-hadoop-emulator-get-started.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hadoop-hdinsight-intro]: hdinsight-hadoop-introduction.md
[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://go.microsoft.com/fwlink/?LinkId=510084
[apache-hive]: http://go.microsoft.com/fwlink/?LinkId=510085
[apache-mapreduce]: http://go.microsoft.com/fwlink/?LinkId=510086
[apache-hdfs]: http://go.microsoft.com/fwlink/?LinkId=510087
[hdinsight-hbase-custom-provision]: hdinsight-hbase-tutorial-get-started.md


[powershell-download]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell-install-configure]: powershell-install-configure.md
[powershell-open]: powershell-install-configure.md#step-1-install


[img-hdi-dashboard]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.png
[img-hdi-dashboard-query-select]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.browse.output.png

[img-hdi-getstarted-video]: ./media/hdinsight-hadoop-tutorial-get-started-windows/hdi-get-started-video.png


[image-hdi-storageaccount-quickcreate]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.StorageAccount.QuickCreate.png
[image-hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.ClusterStatus.png
[image-hdi-quickcreatecluster]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.QuickCreateCluster.png
[image-hdi-getstarted-flow]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GetStartedFlow.png

[image-hdi-gettingstarted-powerquery-importdata]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData.png
[image-hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData2.png
 
