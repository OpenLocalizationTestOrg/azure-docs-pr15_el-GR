<properties
    pageTitle="Γρήγορα αποτελέσματα με το ροή ανάλυση: εντοπισμός σε πραγματικό χρόνο απάτη | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια λύση εντοπισμού απάτη σε πραγματικό χρόνο με ροή ανάλυσης. Χρησιμοποιήστε ένα διανομέα συμβάντων για επεξεργασία συμβάντος σε πραγματικό χρόνο."
    keywords="εντοπισμός ανωμαλία, απάτη εντοπισμού, εντοπισμός ανωμαλία πραγματικό χρόνο"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Γρήγορα αποτελέσματα με το Azure ροή ανάλυση: εντοπισμός απάτη σε πραγματικό χρόνο

Μάθετε πώς μπορείτε να δημιουργήσετε μια λύση για να ολοκληρωμένες για τον εντοπισμό απάτη σε πραγματικό χρόνο με ανάλυση ροή Azure. Φέρτε συμβάντα σε ένα συμβάν Azure διανομέα, να συντάξουν ανάλυσης ροή ερωτήματα για συνάθροιση ή ειδοποίησης και αποστολή των αποτελεσμάτων ενός δέκτη εξόδου για να αποκτήσετε πληροφορίες για στα δεδομένα με την επεξεργασία σε πραγματικό χρόνο. Εντοπισμός ανωμαλία πραγματικό χρόνο για τηλεπικοινωνιακό καλύπτεται αλλά την τεχνική παράδειγμα εξίσου είναι κατάλληλος για άλλους τύπους εντοπισμού απάτη όπως πιστωτική κάρτα ή ταυτότητας κλοπή σενάρια.

Ροή ανάλυσης είναι μια πλήρως διαχειριζόμενη υπηρεσία παρέχοντας επεξεργασία χαμηλής αδράνειας, ιδιαίτερα διαθέσιμες, με σύνθετη συμβάντος μέσω ροής δεδομένων στο cloud. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εισαγωγή στην ανάλυση ροή Azure](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Σενάριο: Τηλεπικοινωνιακό και SIM απάτη ανίχνευση σε πραγματικό χρόνο

Μια εταιρεία τηλεπικοινωνιακό έχει μεγάλο όγκο δεδομένων για εισερχόμενες κλήσεις. Η εταιρεία πρέπει τα ακόλουθα από τα δεδομένα:
* Αυτά τα δεδομένα προς τα κάτω έναν αριθμό διαχειρίσιμα κριση και αποκτήστε ιδέες σχετικά με τη χρήση πελατών μέσα στο χρόνο και γεωγραφική περιοχή.
* Εντοπίζει απάτη SIM (πολλαπλές κλήσεις που προέρχονται από την ίδια ταυτότητα γύρω από την ίδια στιγμή, αλλά σε γεωγραφικά διαφορετικές θέσεις του) σε πραγματικό χρόνο, έτσι ώστε να που μπορούν να απαντήσουν εύκολα με που σας ειδοποιεί τους πελάτες ή τερματισμό της υπηρεσίας.

Σε κανονική σενάρια Internet πράγματα (IoT) υπάρχει ένα τόνος τηλεμετρίας ή αισθητήρα δεδομένων δημιουργείται – και τους πελάτες που θέλετε για να αθροίσετε τους ή ειδοποίηση μέσω ανωμαλίες σε πραγματικό χρόνο.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Κάντε λήψη του [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) από το Κέντρο λήψης της Microsoft 
- Προαιρετικό: Πηγαίου κώδικα από το συμβάν γεννήτρια από [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TelcoGenerator)

## <a name="create-an-azure-event-hubs-input-and-consumer-group"></a>Να δημιουργήσετε μια εισαγωγής Azure συμβάν διανομείς και ομάδα καταναλωτή

Το δείγμα εφαρμογής θα δημιουργήσει συμβάντα και να προωθήσετε τα σε μια παρουσία διανομέα συμβάντων για επεξεργασία σε πραγματικό χρόνο. Υπηρεσία Bus συμβάν διανομείς είναι την προτιμώμενη μέθοδο κατάποσης συμβάντων για ροή ανάλυση και μπορείτε να μάθετε περισσότερα σχετικά με το συμβάν διανομείς στην [τεκμηρίωση Bus υπηρεσίας Azure](/documentation/services/service-bus/).

Για να δημιουργήσετε ένα διανομέα συμβάν:

1.  Στην [πύλη του Azure](https://manage.windowsazure.com/) , κάντε κλικ στην επιλογή **Δημιουργία** > **Εφαρμογή υπηρεσιών** > **Bus υπηρεσίας** > **Διανομέα συμβάν** > **Γρήγορης δημιουργίας**. Δώστε ένα όνομα, περιοχής και νέο ή υπάρχον χώρος ονομάτων για να δημιουργήσετε ένα νέο συμβάν διανομέα.  
2.  Η βέλτιστη πρακτική κάθε ανάλυση ροή εργασίας πρέπει να διαβάσετε από μία μόνο ομάδα καταναλωτή διανομέα συμβάν. Θα σας θα σας καθοδηγήσει στη διαδικασία δημιουργίας μιας ομάδας καταναλωτή παρακάτω και μπορείτε να [Μάθετε περισσότερα σχετικά με τις ομάδες καταναλωτών](https://msdn.microsoft.com/library/azure/dn836025.aspx). Για να δημιουργήσετε μια ομάδα καταναλωτή, μεταβείτε στο ενότητα που μόλις δημιουργήθηκε το συμβάν και κάντε κλικ στην καρτέλα **Ομάδες καταναλωτή** , στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία** στο κάτω μέρος της σελίδας και δώστε ένα όνομα για την ομάδα σας καταναλωτών.
3.  Για να εκχωρήσετε πρόσβαση σε ενότητα συμβάντων, θα πρέπει να δημιουργήσετε μια πολιτική κοινόχρηστη πρόσβαση.  Κάντε κλικ στην καρτέλα **Ρύθμιση παραμέτρων** από το κέντρο του συμβάντος σας.
4.  Στην περιοχή **Πολιτικές πρόσβασης σε κοινή χρήση**, δημιουργήστε μια νέα πολιτική με **Διαχείριση** δικαιωμάτων.

    ![Κοινή χρήση πολιτικές πρόσβασης όπου μπορείτε να δημιουργήσετε μια πολιτική με Διαχείριση δικαιωμάτων.](./media/stream-analytics-get-started/stream-ananlytics-shared-access-policies.png)

5.  Κάντε κλικ στην επιλογή " **Αποθήκευση** " στο κάτω μέρος της σελίδας.
6.  Μεταβείτε στον πίνακα **εργαλείων** και κάντε κλικ στην επιλογή **Πληροφορίες σύνδεσης** στο κάτω μέρος της σελίδας, και στη συνέχεια, αντιγράψτε και αποθηκεύστε τις πληροφορίες σύνδεσης.

## <a name="configure-and-start-event-generator-application"></a>Ρύθμιση παραμέτρων και την έναρξη της εφαρμογής δημιουργίας συμβάντος

Παρέχουμε μια εφαρμογή προγράμματος-πελάτη που θα δημιουργήσετε δείγμα εισερχόμενης κλήσης μεταδεδομένων και push το συμβάν διανομέα. Ακολουθήστε τα παρακάτω βήματα για να ορίσετε αυτήν την εφαρμογή.  

1.  Κάντε λήψη του [αρχείου TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip). Στη συνέχεια, αποσυμπιέστε το σε έναν κατάλογο.

    **Σημείωση**: Windows μπορεί να αποκλείσετε το αρχείο zip που έχετε λάβει. Κάντε δεξί κλικ στο αρχείο και επιλέξτε Ιδιότητες. Εάν το μήνυμα "αυτό το αρχείο προήλθε από έναν άλλο υπολογιστή και μπορεί να έχει αποκλειστεί για την προστασία αυτόν τον υπολογιστή." υποδιαιρέσεις, στη συνέχεια, στο πλαίσιο "Αναίρεση αποκλεισμού" και κάντε κλικ στην επιλογή εφαρμογή στο αρχείο zip.

2.  Αντικαταστήστε τις τιμές Microsoft.ServiceBus.ConnectionString και EventHubName **telcodatagen.exe.config** με το συμβάν διανομέα συμβολοσειρά σύνδεσης και το όνομα.

    **Σημείωση**: τη συμβολοσειρά σύνδεσης που αντιγράψατε από το Azure πύλης τοποθετεί το όνομα της σύνδεσης στο τέλος. Φροντίστε να καταργήσετε τα "; EntityPath =<value>"από το κλειδί Προσθήκη = πεδίο.

3.  Εκκίνηση της εφαρμογής. Η χρήση της είναι ως εξής:

   telcodatagen.exe [#NumCDRsPerHour] [πιθανότητα απάτη κάρτα SIM] [#DurationHours]

Το παρακάτω παράδειγμα θα δημιουργήσει συμβάντων 1000 με πιθανότητα 20 τοις εκατό απάτης στη διάρκεια της 2 ώρες.

    telcodatagen.exe 1000 .2 2

Θα δείτε τις εγγραφές που στέλνεται με το συμβάν διανομέα. Ορισμένα πεδία κλειδιών που θα χρησιμοποιήσουμε σε αυτήν την εφαρμογή εντοπισμού σε πραγματικό χρόνο απάτη ορίζονται εδώ:

| Εγγραφή | Ορισμός |
| ------------- | ------------- |
| CallrecTime | Ώρα έναρξης χρονικής σήμανσης για την κλήση. |
| SwitchNum | Διακόπτης τηλεφώνου που χρησιμοποιούνται για τη σύνδεση της κλήσης. |
| CallingNum | Αριθμός τηλεφώνου του καλούντος. |
| CallingIMSI | Διεθνής συνδρομητής κινητή ταυτότητα (IMSI).  Μοναδικό αναγνωριστικό του καλούντος. |
| CalledNum | Τον αριθμό τηλεφώνου του παραλήπτη της κλήσης. |
| CalledIMSI | Διεθνής συνδρομητής κινητή ταυτότητα (IMSI).  Μοναδικό αναγνωριστικό του παραλήπτη της κλήσης. |


## <a name="create-stream-analytics-job"></a>Δημιουργία αναλυτικών στοιχείων ροής εργασίας
Τώρα που έχουμε μια ροή τηλεπικοινωνιακό συμβάντων, θα σας να ρυθμίσετε μια εργασία ροής ανάλυσης για την ανάλυση αυτά τα συμβάντα σε πραγματικό χρόνο.

### <a name="provision-a-stream-analytics-job"></a>Παροχή μιας ανάλυσης ροής εργασίας

1.  Στην πύλη του Azure, κάντε κλικ στην επιλογή **Δημιουργία > υπηρεσίες δεδομένων > ανάλυση ροή > Γρήγορη δημιουργία**.
2.  Καθορίστε τις παρακάτω τιμές και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία ροή ανάλυσης εργασίας**:

    * **Όνομα εργασίας**: Πληκτρολογήστε ένα όνομα εργασίας.

    * **Περιοχή**: Επιλέξτε την περιοχή όπου θέλετε να εκτελέσετε την εργασία. Εξετάστε το ενδεχόμενο τοποθετώντας το έργο και την ενότητα συμβάντων στην ίδια περιοχή για να εξασφαλίσουν την καλύτερη απόδοση και να διασφαλίσετε ότι θα δεν πληρώνετε για τη μεταφορά δεδομένων μεταξύ περιοχών.

    * **Το λογαριασμό χώρου αποθήκευσης**: Επιλέξτε το λογαριασμό Azure χώρου αποθήκευσης που θέλετε να χρησιμοποιήσετε για την αποθήκευση δεδομένων παρακολούθησης για όλες τις εργασίες ροής ανάλυση εκτελούνται μέσα σε αυτήν την περιοχή. Έχετε την επιλογή για να επιλέξετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης ή για να δημιουργήσετε ένα νέο.

3.  Κάντε κλικ στην επιλογή **Ανάλυσης ροής** στο αριστερό τμήμα του παραθύρου για μια λίστα των εργασιών ροής ανάλυσης.

    ![Εικονίδιο της υπηρεσίας ανάλυσης ροής](./media/stream-analytics-get-started/stream-analytics-service-icon.png)

4.  Το νέο έργο θα εμφανίζεται με κατάσταση **δημιουργήθηκε**. Παρατηρήστε ότι είναι απενεργοποιημένο το κουμπί " **Έναρξη** " στο κάτω μέρος της σελίδας. Πρέπει να ρυθμίσετε την εργασία εισαγωγής, εξόδου και ερωτήματος να μπορέσετε να ξεκινήσετε την εργασία.

### <a name="specify-job-input"></a>Καθορισμός εισαγωγής έργου
1.  Στην εργασία σας ανάλυση ροή κάντε κλικ στην επιλογή **εισόδων** από το επάνω μέρος της σελίδας και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη εισαγωγής**. Παράθυρο διαλόγου που ανοίγει θα καθοδηγήσουμε ορισμένα βήματα για να ρυθμίσετε τα δεδομένα εισόδου σας.
2.  Επιλέξτε **ροής δεδομένων**και, στη συνέχεια, κάντε κλικ στο δεξιό κουμπί.
3.  Επιλέξτε **Διανομέα συμβάν**και, στη συνέχεια, κάντε κλικ στο δεξιό κουμπί.
4.  Πληκτρολογήστε ή επιλέξτε τις παρακάτω τιμές στην τρίτη σελίδα:

    * **Ψευδώνυμο εισαγωγής**: Πληκτρολογήστε ένα φιλικό όνομα για αυτό το έργο εισαγωγής όπως *CallStream*. Σημειώστε ότι πρόκειται να χρησιμοποιήσετε αυτό το όνομα του ερωτήματος αργότερα.
    * **Ενότητα συμβάν**: Εάν το συμβάν διανομέα που δημιουργήσατε είναι στην ίδια συνδρομή ως της ανάλυσης ροής εργασίας, επιλέξτε το χώρο ονομάτων που είναι ο διανομέας συμβάντος στο.

    Εάν το Κέντρο συμβάν σας βρίσκεται σε μια διαφορετική συνδρομή, επιλέξτε **Χρήση διανομέα συμβάν από μια άλλη συνδρομή** και με μη αυτόματο τρόπο εισαγάγετε πληροφορίες για **Namespace Bus υπηρεσίας**, **Όνομα διανομέας συμβάντος**, **Όνομα πολιτικής διανομέα συμβάντος**, **Κλειδί πολιτικής διανομέα συμβάντων**και **Πλήθος διαμερισμάτων διανομέα συμβάν**.

    * **Όνομα συμβάντος διανομέα**: Επιλέξτε το όνομα του συμβάντος διανομέα.

    * **Όνομα πολιτικής διανομέα συμβάν**: Επιλέξτε την πολιτική συμβάν διανομέα που δημιουργήσατε νωρίτερα σε αυτό το πρόγραμμα εκμάθησης.

    * **Ομάδα καταναλωτή διανομέα συμβάν**: Πληκτρολογήστε την ομάδα καταναλωτή που δημιουργήσατε νωρίτερα σε αυτό το πρόγραμμα εκμάθησης.
5.  Κάντε κλικ στο δεξιό κουμπί.
6.  Καθορίστε τις ακόλουθες τιμές:

    * **Μορφή σειριοποιητής συμβάν**: JSON
    * **Κωδικοποίηση**: UTF8
7.  Κάντε κλικ στο κουμπί ελέγχου για να προσθέσετε αυτή την προέλευση και για να επαληθεύσετε ότι ροή ανάλυσης να συνδεθείτε με επιτυχία την ενότητα συμβάντων.

### <a name="specify-job-query"></a>Καθορισμός ερωτήματος εργασία

Ανάλυση ροή υποστηρίζει ένα μοντέλο απλή, δηλωτικό ερωτήματος για την περιγραφή μετασχηματισμοί για επεξεργασία σε πραγματικό χρόνο. Για να μάθετε περισσότερα σχετικά με τη γλώσσα, ανατρέξτε στο άρθρο [Αναφορά γλώσσας ερωτήματος ανάλυσης ροή Azure](https://msdn.microsoft.com/library/dn834998.aspx). Αυτό το πρόγραμμα εκμάθησης, θα σας βοηθήσει να συντάκτη και να ελέγξετε πολλά ερωτήματα μέσω σας σε πραγματικό χρόνο ροή δεδομένων κλήσης.

#### <a name="optional-sample-input-data"></a>Προαιρετικό: Το δείγμα δεδομένων εισόδου
Για να επικυρώσετε το ερώτημά σας σε σχέση με την πραγματική εργασία δεδομένων, μπορείτε να χρησιμοποιήσετε τη δυνατότητα **Δείγματος δεδομένων** για να εξαγάγετε συμβάντα από τη ροή σας και να δημιουργήσετε ένα. Αρχείο JSON τα συμβάντα για σκοπούς δοκιμής.  Ακολουθήστε τα παρακάτω βήματα δείχνουν πώς μπορείτε να το κάνετε αυτό και παρέχουμε επίσης ένα δείγμα αρχείου [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) για σκοπούς δοκιμής.

1.  Επιλέξτε το συμβάν διανομέα εισαγωγής και κάντε κλικ στο **Δείγμα δεδομένων** στο κάτω μέρος της σελίδας.
2.  Στο παράθυρο διαλόγου που εμφανίζεται, καθορίστε **Ώρα έναρξης** για να ξεκινήσετε τη συλλογή δεδομένων από και μια **διάρκεια** για τον όγκο πρόσθετα δεδομένα για την εκμετάλλευση.
3.  Κάντε κλικ στο κουμπί ελέγχου για να ξεκινήσετε δειγματοληψία δεδομένων από την είσοδο.  Αυτό μπορεί να διαρκέσει ένα ή δύο για το αρχείο δεδομένων που θα παραχθεί λεπτά.  Όταν ολοκληρωθεί η διαδικασία, κάντε κλικ στην επιλογή **Λεπτομέρειες** και κάντε λήψη και αποθηκεύστε την. Αρχείο JSON που δημιουργείται.

    ![Λήψη και αποθήκευση επεξεργασία δεδομένων σε ένα αρχείο JSON](./media/stream-analytics-get-started/stream-analytics-download-save-json-file.png)

#### <a name="passthrough-query"></a>Ερώτημα διαβίβασης

Εάν θέλετε να αρχειοθετήσετε κάθε συμβάντος, μπορείτε να χρησιμοποιήσετε ένα ερώτημα διαβίβασης για να διαβάσετε όλα τα πεδία στο φορτίο του συμβάντος ή του μηνύματος. Για να ξεκινήσετε με, κάντε ένα ερώτημα διαβίβασης απλό που έργα όλα τα πεδία του συμβάντος.

1.  Κάντε κλικ στο **ερώτημα** από το επάνω μέρος της σελίδας ανάλυση ροής εργασίας.
2.  Προσθέστε τα ακόλουθα στο πρόγραμμα επεξεργασίας κώδικα:

        SELECT * FROM CallStream

    > Βεβαιωθείτε ότι το όνομα του αρχείου προέλευσης εισαγωγής συμφωνεί με το όνομα της εισόδου που καθορίσατε προηγουμένως.

3.  Στην περιοχή του προγράμματος επεξεργασίας ερωτήματος, κάντε κλικ στην επιλογή **Έλεγχος** .
4.  Παρέχετε ένα αρχείο δοκιμής, οποιοσδήποτε από τους δύο που δημιουργήσατε χρησιμοποιώντας τα προηγούμενα βήματα ή χρησιμοποιήστε [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json).
5.  Κάντε κλικ στο κουμπί έλεγχος και δείτε τα αποτελέσματα εμφανίζονται κάτω από τον ορισμό του ερωτήματος.

    ![Αποτελέσματα ερωτήματος ορισμού](./media/stream-analytics-get-started/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Προβολή στήλης

Τώρα, θα σας θα κριση προς τα κάτω σε ένα μικρότερο σύνολο τα πεδία που επιστρέφονται.

1.  Αλλάξτε το ερώτημα στο πρόγραμμα επεξεργασίας κώδικα για να:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Κάντε κλικ στην επιλογή **Επανεκτέλεση** κάτω από το πρόγραμμα επεξεργασίας ερωτήματος για να δείτε τα αποτελέσματα του ερωτήματος.

    ![Αποτέλεσμα στο πρόγραμμα επεξεργασίας ερωτήματος.](./media/stream-analytics-get-started/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Πλήθος εισερχόμενες κλήσεις ανά περιοχή: παράθυρο Tumbling με συνάθροισης

Για να συγκρίνετε το ποσό που εισερχόμενες κλήσεις ανά περιοχή θα σας θα αξιοποιήσετε μια [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) για να λάβετε το πλήθος των εισερχόμενων κλήσεων ομαδοποιημένα κατά SwitchNum κάθε 5 δευτερόλεπτα.

1.  Αλλάξτε το ερώτημα στο πρόγραμμα επεξεργασίας κώδικα για να:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Αυτό το ερώτημα χρησιμοποιεί τη **Χρονική σήμανση από** τη λέξη-κλειδί για να καθορίσετε ένα πεδίο χρονικής σήμανσης στο φορτίο να χρησιμοποιηθεί με το χρονικό κατά τον υπολογισμό. Εάν αυτό το πεδίο δεν καθορίζεται, τη λειτουργία Άνοιγμα παραθύρου θα εκτελεστεί χρησιμοποιώντας το χρόνο κάθε συμβάντος έφτασαν διανομέα συμβάν. Ανατρέξτε στο θέμα [αναφορά γλώσσας ερωτήματος "Άφιξη ώρα και στο εφαρμογής ώρα" σε την ανάλυση ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Σημειώστε ότι έχετε πρόσβαση σε μια χρονική σήμανση για το τέλος κάθε παραθύρου, χρησιμοποιώντας την ιδιότητα **System.Timestamp** .

2.  Κάντε κλικ στην επιλογή **Επανεκτέλεση** κάτω από το πρόγραμμα επεξεργασίας ερωτήματος για να δείτε τα αποτελέσματα του ερωτήματος.

    ![Αποτελέσματα ερωτήματος για Timestand από](./media/stream-analytics-get-started/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>Εντοπισμός απάτη SIM με ιδιο-συνδέσμου

Για να προσδιορίσετε πιθανώς απάτες χρήση θα δούμε για κλήσεις που προέρχονται από τον ίδιο χρήστη, αλλά σε διαφορετικές θέσεις σε λιγότερο από 5 δευτερόλεπτα.  Θα σας [συμμετοχή](https://msdn.microsoft.com/library/azure/dn835026.aspx) στη ροή συμβάντων κλήσης με το ίδιο για να ελέγξετε για αυτές τις περιπτώσεις.

1.  Αλλάξτε το ερώτημα στο πρόγραμμα επεξεργασίας κώδικα για να:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Κάντε κλικ στην επιλογή **Επανεκτέλεση** κάτω από το πρόγραμμα επεξεργασίας ερωτήματος για να δείτε τα αποτελέσματα του ερωτήματος.

    ![Αποτελέσματα αναζήτησης του συνδέσμου](./media/stream-analytics-get-started/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Δημιουργία δέκτη εξόδου

Τώρα που έχετε ορίσαμε ροή ενός συμβάντος, ένα συμβάν διανομέα της εισόδου ingest συμβάντων και ένα ερώτημα για να εκτελέσετε ένα μετασχηματισμό από τη ροή, το τελευταίο βήμα είναι να ορίσετε ενός δέκτη εξόδου για το έργο.  Θα σας θα εγγραφή συμβάντων για ως δόλια συμπεριφορά στο χώρο αποθήκευσης αντικειμένων Blob.

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε ένα κοντέινερ για το χώρο αποθήκευσης αντικειμένων Blob, εάν δεν έχετε ήδη ένα.

1.  Χρήση ενός υπάρχοντος λογαριασμού χώρου αποθήκευσης ή να δημιουργήσετε ένα νέο λογαριασμό του χώρου αποθήκευσης, κάνοντας κλικ **ΔΗΜΙΟΥΡΓΊΑ > ΥΠΗΡΕΣΊΕΣ ΔΕΔΟΜΈΝΩΝ > ΑΠΟΘΉΚΕΥΣΗ > ΓΡΉΓΟΡΗ ΔΗΜΙΟΥΡΓΊΑ** και ακολουθώντας τις οδηγίες.
2.  Επιλέξτε το λογαριασμό χώρου αποθήκευσης, κάντε κλικ στην επιλογή **ΚΟΝΤΈΙΝΕΡ** στο επάνω μέρος της σελίδας και, στη συνέχεια, κάντε κλικ στην επιλογή **ΠΡΟΣΘΉΚΗ**.
3.  Καθορίστε ένα **ΌΝΟΜΑ** για το κοντέινερ και ορίστε την **ΠΡΌΣΒΑΣΗ** σε δημόσια Blob.

## <a name="specify-job-output"></a>Καθορίστε το αποτέλεσμα του έργου

1.  Στην εργασία σας ανάλυση ροή κάντε κλικ στην επιλογή **ΈΞΟΔΟΣ** από το επάνω μέρος της σελίδας και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη ΕΞΌΔΟΥ**. Παράθυρο διαλόγου που ανοίγει θα καθοδηγήσουμε ορισμένα βήματα για να ρυθμίσετε το αποτέλεσμα.
2.  Επιλέξτε **Χώρο ΑΠΟΘΉΚΕΥΣΗΣ αντικειμένων BLOB**και, στη συνέχεια, κάντε κλικ στο δεξιό κουμπί.
3.  Πληκτρολογήστε ή επιλέξτε τις παρακάτω τιμές στην τρίτη σελίδα:

    * **ΨΕΥΔΏΝΥΜΟ ΕΞΌΔΟΥ**: Πληκτρολογήστε ένα φιλικό όνομα για αυτό το αποτέλεσμα εργασίας.
    * **ΣΥΝΔΡΟΜΉ**: Εάν το χώρο αποθήκευσης αντικειμένων Blob που δημιουργήσατε είναι στην ίδια συνδρομή ως της ανάλυσης ροή εργασίας, επιλέξτε **Χρήση λογαριασμού χώρου αποθήκευσης από τρέχουσα συνδρομή σας**. Εάν ο χώρος αποθήκευσης είναι σε μια διαφορετική συνδρομή, επιλέξτε **Χρήση λογαριασμού χώρου αποθήκευσης από μια άλλη συνδρομή** και εισαγάγετε με μη αυτόματο τρόπο τις πληροφορίες για **Το ΛΟΓΑΡΙΑΣΜΌ χώρου ΑΠΟΘΉΚΕΥΣΗΣ**, **ΚΛΕΙΔΊ ΛΟΓΑΡΙΑΣΜΟΎ χώρου ΑΠΟΘΉΚΕΥΣΗΣ**, **ΚΟΝΤΈΙΝΕΡ**.
    * **Το ΛΟΓΑΡΙΑΣΜΌ χώρου ΑΠΟΘΉΚΕΥΣΗΣ**: Επιλέξτε το όνομα του λογαριασμού χώρου αποθήκευσης.
    * **ΚΟΝΤΈΙΝΕΡ**: Επιλέξτε το όνομα του κοντέινερ.
    * **ΠΡΌΘΕΜΑ FILENAME**: Πληκτρολογήστε ένα αρχείο πρόθεμα για χρήση κατά τη σύνταξη blob εξόδου.

4.  Κάντε κλικ στο δεξιό κουμπί.
5.  Καθορίστε τις ακόλουθες τιμές:

    * **ΜΟΡΦΉ ΣΕΙΡΙΟΠΟΙΗΤΉΣ ΣΥΜΒΆΝ**: JSON
    * **ΚΩΔΙΚΟΠΟΊΗΣΗ**: UTF8

6.  Κάντε κλικ στο κουμπί ελέγχου για να προσθέσετε αυτή την προέλευση και για να επαληθεύσετε ότι ανάλυση ροή μπορεί να συνδεθεί με επιτυχία με το λογαριασμό χώρου αποθήκευσης.

## <a name="start-job-for-real-time-processing"></a>Έναρξη εργασίας για επεξεργασία πραγματικό χρόνο

Εφόσον μια εργασία εισαγωγής ερωτήματος και εξόδου όλα έχουν καθοριστεί, είμαστε έτοιμοι να ξεκινήσει η εργασία ροής ανάλυσης για τον εντοπισμό απάτη σε πραγματικό χρόνο.

1.  Από την εργασία **πίνακα ΕΡΓΑΛΕΊΩΝ**, κάντε κλικ στο κουμπί " **ΈΝΑΡΞΗ** " στο κάτω μέρος της σελίδας.
2.  Στο παράθυρο διαλόγου που εμφανίζεται, επιλέξτε **ΏΡΑ ΈΝΑΡΞΗΣ του ΈΡΓΟΥ** και, στη συνέχεια, κάντε κλικ στο κουμπί ελέγχου στο κάτω μέρος του παραθύρου διαλόγου. Την κατάσταση της εργασίας θα αλλάξει σε **Έναρξη** και σύντομα θα μετακινηθεί για να **εκτελείται**.

## <a name="view-fraud-detection-output"></a>Προβολή απάτη εντοπισμού εξόδου

Χρησιμοποιήστε ένα εργαλείο όπως [Εξερεύνηση χώρου αποθήκευσης Azure](https://azurestorageexplorer.codeplex.com/) ή [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) για να προβάλετε ως δόλια συμβάντα όπως γίνεται εγγραφή σε το αποτέλεσμα σε πραγματικό χρόνο.  

![Εντοπισμός απάτη: ως δόλια συμβάντα προβληθεί σε πραγματικό χρόνο](./media/stream-analytics-get-started/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Λήψη υποστήριξης
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ Azure ροή ανάλυσης](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)
