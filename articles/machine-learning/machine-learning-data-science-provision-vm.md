<properties 
    pageTitle="Προμήθεια εικονική μηχανή της Microsoft δεδομένα Science | Microsoft Azure" 
    description="Ρύθμιση παραμέτρων και δημιουργήστε μια εικονική μηχανή Science δεδομένων στον Azure για να κάνετε ανάλυση και εκμάθηση του υπολογιστή." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="bradsev" />


# <a name="provision-the-microsoft-data-science-virtual-machine"></a>Προμήθεια η εικονική μηχανή Science δεδομένων της Microsoft

Η εικονική μηχανή Microsoft δεδομένων Science είναι μια εικόνα Azure εικονική μηχανή (Εικονική) προ-εγκατασταθεί και ρυθμιστεί με πολλές δημοφιλείς εργαλεία που χρησιμοποιούνται ευρέως για ανάλυση δεδομένων και μηχανικής εκμάθησης. Τα εργαλεία που περιλαμβάνονται είναι οι εξής:

- Έκδοση προγραμματιστή διακομιστή Microsoft R
- Κατανομή Python anaconda
- Σημειωματάριο Jupyter (με R, πυρήνων Python)
- Edition Κοινότητας Visual Studio
- Επιφάνεια εργασίας του Power BI
- Ο SQL Server 2016 για προγραμματιστές Edition
- Μηχανολογικά εργαλεία εκμάθησης
    - [Κιτ εργαλείων υπολογιστική δικτύου (CNTK)](https://github.com/Microsoft/CNTK): ένα βάθος εκμάθησης Κιτ εργαλείων λογισμικού από τη Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): μια γρήγορη μηχανικής εκμάθησης σύστημα υποστήριξης τεχνικές όπως σύνδεση, κλειδώματος, allreduce, μειώσεων, learning2search, ενεργές, και αλληλεπιδραστικών εκμάθηση.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): ένα εργαλείο παρέχοντας υλοποίηση γρήγορα και ακριβή ενισχύεται δέντρου.
    - [Rattle](http://rattle.togaware.com/) (το R αναλυτικής εργαλείο για να μάθετε εύκολα): ένα εργαλείο που κάνει γρήγορα αποτελέσματα με την ανάλυση δεδομένων και μηχανικής εκμάθησης στο R εύκολη, με την Εξερεύνηση δεδομένων με βάση Γραφικών και μοντελοποίησης με αυτόματη δημιουργία κώδικα R.
    - [mxnet](https://github.com/dmlc/mxnet): ένα πλαίσιο βαθύ εκμάθησης που έχει σχεδιαστεί για την απόδοση και ευελιξία
- Βιβλιοθήκες σε R και Python για χρήση σε Azure μηχανικής εκμάθησης και άλλες υπηρεσίες του Azure
- Git, συμπεριλαμβανομένων των Git πάρτι για να εργαστείτε με αποθετήρια κώδικα προέλευσης όπως GitHub, Visual Studio Team Services
- Θύρες Windows από πολλές δημοφιλείς Linux βοηθητικά προγράμματα γραμμής εντολών (συμπεριλαμβανομένων των awk, sed, perl, grep, εύρεση, wget, καμπύλη κ.λπ) προσβάσιμα μέσω εντολών. 


Κάνοντας science δεδομένων περιλαμβάνει διαδοχικών προσεγγίσεων σε μια σειρά από εργασίες: εύρεση, φόρτωση και προ-επεξεργασίας δεδομένων, δημιουργία και δοκιμές μοντέλα και την ανάπτυξη των μοντέλων για κατανάλωση σε έξυπνες εφαρμογές. Δεδομένα επιστήμονες Χρησιμοποιήστε μια ποικιλία εργαλείων για να ολοκληρώσετε αυτές τις εργασίες. Μπορεί να είναι αρκετά χρονοβόρα, για να βρείτε τις κατάλληλες εκδόσεις του λογισμικού, και, στη συνέχεια, κάντε λήψη και εγκαταστήστε τα. Η εικονική μηχανή Microsoft δεδομένων Science μπορεί να μειώσει αυτής της επιβάρυνσης, παρέχοντας μια εικόνα έτοιμη για χρήση που μπορεί να καθοριστεί σε Azure με όλα τα πολλά εργαλεία δημοφιλείς προ-εγκατασταθεί και ρυθμιστεί. 

Η εικονική μηχανή Microsoft δεδομένων φυσικής jump-starts το έργο σας ανάλυσης. Σας επιτρέπει σε εργασίες σε διάφορες γλώσσες, συμπεριλαμβανομένων των R, Python, SQL και C#. Visual Studio παρέχει IDE για να αναπτύξετε και να ελέγξετε τον κωδικό που είναι εύκολο για χρήση. Το SDK Azure που περιλαμβάνονται σε η Εικονική σάς επιτρέπει να δημιουργήσετε τις εφαρμογές σας χρησιμοποιώντας τις διάφορες υπηρεσίες σε πλατφόρμα cloud της Microsoft. 

Υπάρχουν χωρίς χρεώσεις λογισμικού για αυτήν την εικόνα Εικονική science δεδομένων. Μόνο πληρώνετε για τις χρεώσεις Azure χρήση ποια εξαρτάται από το μέγεθος του η εικονική μηχανή που παρέχετε. Μπορείτε να βρείτε περισσότερες λεπτομέρειες σχετικά με τις χρεώσεις υπολογισμού στην ενότητα λεπτομέρειες Τιμολόγηση στη σελίδα [δεδομένων Science εικονική μηχανή](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) . 


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Μπορείτε να δημιουργήσετε μια εικονική μηχανή Microsoft δεδομένων φυσικής, πρέπει να έχετε τα εξής:

- **Συνδρομή του Azure**: για να αποκτήσετε ένα, ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **Το λογαριασμό χώρου αποθήκευσης του Azure**: για να δημιουργήσετε μια λίστα, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού Azure χώρου αποθήκευσης](storage-create-storage-account.md#create-a-storage-account). Εναλλακτικά, το λογαριασμό χώρου αποθήκευσης μπορούν να δημιουργηθούν ως μέρος της διαδικασίας δημιουργία η Εικονική, εάν δεν θέλετε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό.


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Δημιουργήστε την εικονική μηχανή σας Science δεδομένων της Microsoft

Ακολουθούν τα βήματα για να δημιουργήσετε μια παρουσία των δεδομένων Science εικονική μηχανή της Microsoft:

1.  Μεταβείτε στην εικονική μηχανή καταχώρηση στην [πύλη του Azure](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2.   Επιλέξτε το κουμπί **Δημιουργία** στο κάτω μέρος για να μεταβείτε σε έναν οδηγό. ![ρύθμιση παραμέτρων-δεδομένων-επιστήμης-εικονική](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   Ο οδηγός χρησιμοποιούνται για τη δημιουργία η εικονική μηχανή Microsoft δεδομένων Science απαιτεί **εισόδων** για κάθε ένα από τα **πέντε βήματα** απαρίθμηση στα δεξιά της αυτό το σχήμα. Εδώ θα βρείτε τα δεδομένα εισόδου χρειάζεται, για να ρυθμίσετε τις παραμέτρους κάθε ένα από τα εξής βήματα:
    
    1.   **Βασικά στοιχεία**
        1.   **Όνομα**: όνομα του διακομιστή σας science δεδομένων που δημιουργείτε.
        2.   **Όνομα χρήστη**: αναγνωριστικό σύνδεσης λογαριασμό διαχειριστή.
        3.   **Κωδικός πρόσβασης**: κωδικό πρόσβασης του λογαριασμού διαχειριστή.
        4.   **Συνδρομή**: Εάν έχετε περισσότερες από μία συνδρομές, επιλέξτε αυτήν στην οποία η μηχανή είναι να δημιουργηθεί και να τιμολογείται.
        5.   **Ομάδα πόρων**: μπορείτε να δημιουργήσετε ένα νέο ή να χρησιμοποιήσετε μια υπάρχουσα ομάδα.
        6.   **Θέση**: Επιλέξτε το κέντρο δεδομένων που είναι πιο κατάλληλη. Συνήθως είναι το κέντρο δεδομένων που έχει περισσότερα από τα δεδομένα σας ή να είναι πιο κοντά σας φυσική θέση για πιο ασφαλής πρόσβαση στο δίκτυο.
         
    2.   **Μέγεθος**: Επιλέξτε έναν από τους τύπους διακομιστή που ικανοποιεί το λειτουργικών απαιτήσεων και περιορισμών κόστους. Μπορείτε να λάβετε περισσότερες επιλογές Εικονική μεγεθών, επιλέγοντας "Προβολή όλων".
    
    3.   **Ρυθμίσεις**:
        1.   **Τύπος δίσκου**: Επιλέξτε Premium Εάν προτιμάτε μια μονάδα δίσκου οπτικοί (SSD), αλλιώς επιλέξτε "Τυπική".
        2.   **Το λογαριασμό χώρου αποθήκευσης**: να δημιουργήσετε ένα νέο λογαριασμό Azure χώρο αποθήκευσης στη συνδρομή σας είτε να χρησιμοποιήσετε ένα υπάρχον στην ίδια *θέση* που επιλέχθηκε στο βήμα **βασικά στοιχεία** του οδηγού.
        3.   **Οι υπόλοιπες παράμετροι**: συνήθως μπορείτε απλώς να χρησιμοποιήσετε τις προεπιλεγμένες τιμές. Μπορείτε να δείκτη του ποντικιού επάνω στη σύνδεση ενημερωτικό για βοήθεια σχετικά με τα συγκεκριμένα πεδία σε περίπτωση που θέλετε να λάβετε υπόψη τη χρήση μη προεπιλεγμένων τιμών.

    4.   **Σύνοψη**: Βεβαιωθείτε ότι όλες οι πληροφορίες που εισαγάγατε είναι σωστά.
    5.   **Αγορά**: κάντε κλικ στην επιλογή **αγορά** για να ξεκινήσετε την προμήθεια του. Παρέχεται μια σύνδεση με τους όρους της κίνησης. Η Εικονική δεν έχει τυχόν πρόσθετες χρεώσεις πέρα από το υπολογισμού για το μέγεθος του διακομιστή που επιλέξατε στο βήμα **μέγεθος** . 


>[AZURE.NOTE] Την προμήθεια πρέπει να εκτελεί περίπου 10-20 λεπτά. Την κατάσταση της την προμήθεια εμφανίζεται στην πύλη του Azure.

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Πώς να αποκτήσετε πρόσβαση σε δεδομένα Science η εικονική μηχανή

Όταν δημιουργηθεί η Εικονική, μπορείτε να απομακρυσμένης επιφάνειας εργασίας σε αυτήν χρησιμοποιώντας τα διαπιστευτήρια λογαριασμού διαχειριστή που έχετε ρυθμίσει τις παραμέτρους στην προηγούμενη ενότητα **βασικά στοιχεία** . 

Όταν σας Εικονική δημιουργείται και παροχή της υπηρεσίας, είστε έτοιμοι να ξεκινήσετε να χρησιμοποιείτε τα εργαλεία που είναι εγκατεστημένη και ρυθμισμένη σε αυτό. Υπάρχουν πλακίδια μενού Έναρξη και εικονίδια της επιφάνειας εργασίας για πολλά από τα εργαλεία. 

## <a name="how-to-create-a-strong-password-on-the-jupyter-notebook-server"></a>Πώς μπορείτε να δημιουργήσετε έναν ισχυρό κωδικό πρόσβασης στο διακομιστή Jupyter σημειωματαρίου 

Για να δημιουργήσετε το δικό σας ισχυρό κωδικό πρόσβασης για το διακομιστή Σημειωματάριο Jupyter εγκατεστημένο στον υπολογιστή, εκτελέστε την ακόλουθη εντολή από μια γραμμή εντολών στον υπολογιστή εικονικές Science δεδομένων.

    c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Επιλέξτε έναν ισχυρό κωδικό πρόσβασης όταν σας ζητηθεί.

Μπορείτε να δείτε ο κατακερματισμός κωδικού πρόσβασης στη μορφή "sha1:xxxxxx" στο αποτέλεσμα. Αντιγράψτε αυτό κατακερματισμός τον κωδικό πρόσβασης και να αντικαταστήσετε το υπάρχον κατακερματισμός που βρίσκεται στο αρχείο ρύθμισης παραμέτρων του σημειωματαρίου που βρίσκεται στο: **C:\ProgramData\jupyter\jupyter_notebook_config.py** με ένα όνομα παραμέτρου ***c.NotebookApp.password***.

Αντικαταστήστε μόνο το τμήμα (xxxxxx) στην υπάρχουσα τιμή κατακερματισμός που είναι εντός των εισαγωγικών. Τα εισαγωγικά και το ***sha1:*** πρόθεμα για την τιμή της παραμέτρου και τα δύο πρέπει να διατηρηθούν.

Τέλος, πρέπει να διακοπή και επανεκκίνηση του διακομιστή Jupyter, που λειτουργεί με το Εικονική ως windows προγραμματισμένη εργασία που ονομάζεται **Start_IPython_Notebook**. Εάν δεν γίνεται αποδεκτή τον νέο κωδικό πρόσβασης μετά την επανεκκίνηση του αυτήν την εργασία, δοκιμάστε θανάτωση όλα τα python διεργασίες που εκτελούνται από τη Διαχείριση εργασιών και, στη συνέχεια, επανεκκινήστε την προγραμματισμένη εργασία. Εναλλακτικά, δοκιμάστε να κάνετε επανεκκίνηση του υπολογιστή εικονική.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Εργαλεία εγκατασταθεί σε δεδομένα Science εικονική μηχανή της Microsoft

### <a name="microsoft-r-server-developer-edition"></a>Έκδοση προγραμματιστή διακομιστή Microsoft R
Εάν θέλετε να χρησιμοποιήσετε R για την ανάλυση, η Εικονική έχει εγκατεστημένη την έκδοση Microsoft Developer διακομιστή R. Microsoft R Server είναι μια πλατφόρμα ανάλυση ευρέως ανάπτυξης επιχειρηματικής κατηγορίας με βάση R που υποστηρίζεται μεταβλητού μεγέθους και ασφαλή. Υποστήριξη διάφορα στατιστικά στοιχεία μεγάλο δεδομένων, πρόβλεψης μοντελοποίηση και μηχανικής εκμάθησης δυνατότητες, R διακομιστή υποστηρίζει το πλήρες εύρος των ανάλυσης – Εξερεύνηση, ανάλυσης, απεικόνιση και μοντελοποίησης. Χρησιμοποιώντας και επέκταση Άνοιγμα αρχείου προέλευσης R, Microsoft R Server είναι πλήρως συμβατό με τις δέσμες ενεργειών R, συναρτήσεις και πακέτα CRAN, για την ανάλυση δεδομένων σε κλίμακα για μεγάλες επιχειρήσεις. Διευθύνσεις επίσης οι περιορισμοί στη μνήμη Άνοιγμα R προέλευσης, προσθέτοντας παράλληλες και τμηματική επεξεργασία δεδομένων. Αυτό σας επιτρέπει να εκτελέσετε ανάλυση δεδομένα πολύ μεγαλύτερο από τι χωρά στην κύρια μνήμη.  Visual Studio Κοινότητας Edition περιλαμβάνονται στην η Εικονική περιέχει τα εργαλεία R για επέκταση Visual Studio που παρέχει μια πλήρη IDE για την εργασία με R. Επίσης, μπορείτε να κάνετε λήψη και να χρησιμοποιήσετε άλλες IDEs καθώς και όπως [RStudio](http://www.rstudio.com). 

### <a name="python"></a>Python
Για την ανάπτυξη χρησιμοποιώντας Python, έχει εγκατασταθεί Anaconda Python διανομής 2.7 και 3.5. Αυτή η κατανομή περιέχει τη βασική Python μαζί με 300 σχετικά με τα πιο δημοφιλή πακέτα ανάλυση μαθηματικές πράξεις, μηχανικής και δεδομένων. Μπορείτε να χρησιμοποιήσετε εργαλεία Python για Visual Studio (PTVS) που είναι εγκατεστημένο το Visual Studio 2015 Κοινότητας edition ή ένα από τα IDEs μαζί με Anaconda όπως σε ΑΔΡΆΝΕΙΑ ή Spyder. Μπορείτε να εκκινήσετε το ένα από τα εξής κάνοντας αναζήτηση στη γραμμή αναζήτησης (**Win** + πλήκτρο**S** ).

>[AZURE.NOTE] Για να καταδείξετε τα εργαλεία Python για το Visual Studio στο Anaconda Python 2.7 και 3.5, πρέπει να δημιουργήσετε προσαρμοσμένα περιβάλλοντα για κάθε έκδοση. Για να ορίσετε αυτές τις διαδρομές περιβάλλον της έκδοσης Κοινότητας του Visual Studio 2015, μεταβείτε σε **Εργαλεία** -> **Εργαλεία Python** -> **Python περιβάλλοντα** και, στη συνέχεια, κάντε κλικ στο κουμπί **+ προσαρμοσμένη**. 

Anaconda Python 2.7 εγκαθίσταται στην περιοχή C:\Anaconda και έχει εγκατασταθεί Anaconda Python 3,5 στην περιοχή c:\Anaconda\envs\py35. Για λεπτομερείς οδηγίες, ανατρέξτε στο θέμα [τεκμηρίωση PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) . 

### <a name="jupyter-notebook"></a>Jupyter σημειωματαρίου
Κατανομή anaconda περιλαμβάνει επίσης ένα σημειωματάριο Jupyter, ένα περιβάλλον για την κοινή χρήση κώδικα και ανάλυση. Ένα διακομιστής Σημειωματάριο Jupyter έχει ρυθμιστεί εκ των προτέρων με Python 2, Python 3 και πυρήνων R. Υπάρχει ένα εικονίδιο επιφάνειας εργασίας με το όνομα "Jupyter σημειωματάριο για να ξεκινήσετε το πρόγραμμα περιήγησης για να αποκτήσετε πρόσβαση στο διακομιστή του σημειωματαρίου. Εάν είστε σε η Εικονική μέσω απομακρυσμένης επιφάνειας εργασίας, μπορείτε επίσης να επισκεφθείτε [https://localhost:9999 /](https://localhost:9999/) για να αποκτήσετε πρόσβαση στο διακομιστή Σημειωματάριο Jupyter όταν είστε συνδεδεμένοι με το Εικονική.
 
>[AZURE.NOTE] Συνεχίστε εάν λαμβάνετε τυχόν προειδοποιήσεις πιστοποιητικού. 

Θα σας έχουν συσκευαστούν δείγμα σημειωματάρια στο Python και R. Τα σημειωματάρια Jupyter δείχνουν πώς μπορείτε να εργαστείτε με Microsoft R Server, SQL Server 2016 R Services (ανάλυση στη βάση δεδομένων), Python και άλλες τεχνολογίες Azure μόλις συνδεθείτε στον Jupyter. Μπορείτε να δείτε τη σύνδεση με τα δείγματα στην αρχική σελίδα του σημειωματαρίου μετά τον έλεγχο ταυτότητας για το Σημειωματάριο Jupyter χρησιμοποιώντας τον κωδικό πρόσβασης που δημιουργήσατε σε ένα προηγούμενο βήμα. 

### <a name="visual-studio-2015-community-edition"></a>Visual Studio 2015 Κοινότητας edition
Visual Studio Κοινότητας εγκατεστημένη την έκδοση στον η Εικονική. Είναι μια δωρεάν έκδοση του δημοφιλείς IDE από τη Microsoft που μπορείτε να χρησιμοποιήσετε για λόγους αξιολόγησης και για τις μικρές ομάδες. Μπορείτε να ανατρέξετε τους όρους της άδειας χρήσης [εδώ](https://www.visualstudio.com/support/legal/mt171547).  Ανοίξτε το Visual Studio, κάνοντας διπλό κλικ στο εικονίδιο επιφάνειας εργασίας ή το μενού **Έναρξη** . Μπορείτε επίσης να αναζητήσετε προγράμματα με **Win** + **S** και καταχώρηση "Visual Studio". Όταν μπορείτε να δημιουργήσετε έργα σε γλώσσες όπως C#, Python, R, node.js. Οι προσθήκες εγκαθίστανται επίσης που να είναι πιο εύκολο να εργαστείτε με Azure υπηρεσίες όπως Azure καταλόγου δεδομένων, Azure HDInsight (Hadoop, τους) και Azure λίμνης δεδομένων. 

>[AZURE.NOTE] Ενδέχεται να λάβετε ένα μήνυμα που αναφέρει ότι έχει λήξει η περίοδος αξιολόγησης. Εισαγάγετε τα διαπιστευτήριά σας λογαριασμό Microsoft ή δημιουργήστε ένα νέο δωρεάν λογαριασμό για να αποκτήσετε πρόσβαση σε την οπτική Edition Κοινότητας Studio. 

### <a name="sql-server-2016-developer-edition"></a>Έκδοση SQL Server 2016 για προγραμματιστές
Παρέχεται μια έκδοση για προγραμματιστές του SQL Server 2016 με τις υπηρεσίες R για να εκτελέσετε ανάλυση σε βάση δεδομένων σε η Εικονική. Υπηρεσίες R παρέχουν μια πλατφόρμα για την ανάπτυξη και την ανάπτυξη εφαρμογών έξυπνη. Για να δημιουργήσετε μοντέλα και Δημιουργία προβλέψεων για τα δεδομένα του SQL Server, μπορείτε να χρησιμοποιήσετε τη γλώσσα R εμπλουτισμένου και ισχυρές και τα πακέτα πολλά από την Κοινότητα. Μπορείτε να διατηρήσετε ανάλυση κοντά στα δεδομένα επειδή R υπηρεσιών (στο-βάση δεδομένων) ενοποίηση τη γλώσσα R με SQL Server. Αυτό καταργεί το κόστος και τους κινδύνους ασφαλείας που σχετίζονται με κίνηση δεδομένων.

>[AZURE.NOTE] Το SQL Server 2016 edition προγραμματιστής μπορεί να χρησιμοποιηθεί μόνο για σκοπούς δοκιμής ανάπτυξης και. Χρειάζεστε μια άδεια χρήσης για να εκτελέσετε στο παραγωγής. 

Μπορείτε να αποκτήσετε πρόσβαση στον SQL server κατά την εκκίνηση του **SQL Server Management Studio**. Το όνομα Εικονική συμπληρώνεται ως το όνομα του διακομιστή. Χρήση του ελέγχου ταυτότητας των Windows όταν έχετε συνδεθεί ως διαχειριστής του στα Windows. Μόλις συνδεθείτε στο SQL Server Management Studio μπορείτε να δημιουργήσετε άλλους χρήστες, να δημιουργήσετε βάσεις δεδομένων, εισαγωγή δεδομένων, και εκτέλεση ερωτημάτων SQL. 

Για να ενεργοποιήσετε την ανάλυση σε βάση δεδομένων με Microsoft R, εκτελέστε την ακόλουθη εντολή ως ένα χρόνο ενεργειών στο SQL Server management studio μετά την καταγραφή στο ως διαχειριστής του διακομιστή. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure 
Διάφορα εργαλεία Azure είναι εγκατεστημένα στον η Εικονική:

- Υπάρχει μια συντόμευση επιφάνειας εργασίας για να αποκτήσετε πρόσβαση στην τεκμηρίωση Azure SDK. 
- **AzCopy**: χρησιμοποιούνται για τη μετακίνηση δεδομένων και έξοδος από το λογαριασμό σας Microsoft Azure χώρου αποθήκευσης. Για να δείτε χρήση, πληκτρολογήστε **Azcopy** σε μια γραμμή εντολών για να δείτε τη χρήση. 
- **Εξερεύνηση χώρου αποθήκευσης του Microsoft Azure**: χρησιμοποιείται για να περιηγηθείτε σε τα αντικείμενα που έχετε αποθηκεύσει εντός του Azure λογαριασμού χώρου αποθήκευσης και να μεταφέρετε δεδομένα από Azure χώρου αποθήκευσης και. Μπορείτε να πληκτρολογήσετε **Εξερεύνηση χώρου αποθήκευσης** στο πλαίσιο Αναζήτηση ή να το βρείτε στο μενού Έναρξη των Windows για να αποκτήσετε πρόσβαση σε αυτό το εργαλείο. 
- **Adlcopy**: χρησιμοποιείται για να μεταφέρετε δεδομένα Azure λίμνης δεδομένων. Για να δείτε χρήση, πληκτρολογήστε **adlcopy** σε μια γραμμή εντολών. 
- **dtui**: χρησιμοποιούνται για τη μετακίνηση δεδομένων από και προς DocumentDB Azure, μια βάση δεδομένων NoSQL στο cloud. Πληκτρολογήστε **dtui** στη γραμμή εντολών. 
- **Πύλη διαχείρισης δεδομένων της Microsoft**: επιτρέπει την κυκλοφορία δεδομένων μεταξύ των προελεύσεων δεδομένων εσωτερικής εγκατάστασης και cloud. Χρησιμοποιείται μέσα σε εργαλεία όπως το Azure εργοστασίου δεδομένων. 
- **Microsoft Azure Powershell**: ένα εργαλείο που χρησιμοποιείται για τη Διαχείριση Azure τους πόρους σας το Powershell γλώσσας δέσμης ενεργειών είναι επίσης εγκατεστημένη σε Εικονική σας. 

###<a name="power-bi"></a>Power BI

Για να σας βοηθήσει να δημιουργήσετε πίνακες εργαλείων και εξαιρετική απεικονίσεις, έχει εγκατασταθεί το **Power BI Desktop** . Χρησιμοποιήστε αυτό το εργαλείο για να αντλήσει δεδομένα από διαφορετικές προελεύσεις, για τη σύνταξη πινάκων εργαλείων και στις αναφορές σας και να τις δημοσιεύσετε στο cloud. Για πληροφορίες, ανατρέξτε στην τοποθεσία [Power BI](http://powerbi.microsoft.com) . Μπορείτε να βρείτε επιφάνειας εργασίας Power BI στο μενού Έναρξη. 

>[AZURE.NOTE] Χρειάζεστε ένα λογαριασμό Office 365 για πρόσβαση στο Power BI. 

## <a name="additional-microsoft-development-tools"></a>Πρόσθετα εργαλεία ανάπτυξης του Microsoft
Το [**Πρόγραμμα εγκατάστασης του Microsoft Web πλατφόρμα**](https://www.microsoft.com/web/downloads/platform.aspx) μπορεί να χρησιμοποιηθεί για τον εντοπισμό και λήψη άλλα εργαλεία ανάπτυξης της Microsoft. Υπάρχει επίσης μια συντόμευση για το εργαλείο που παρέχεται στην επιφάνεια εργασίας φυσικής εικονική μηχανή Microsoft δεδομένων.  

## <a name="important-directories-on-the-vm"></a>Σημαντικό καταλόγων σε την εικονική Μηχανή

| Στοιχείο                          | Καταλόγου |
| ------------------------------| ---------------- |
|Ρυθμίσεις παραμέτρων διακομιστή Σημειωματάριο Jupyter | C:\ProgramData\jupyter |
|Σημειωματάριο Jupyter δείγματα αρχικού καταλόγου| c:\dsvm\notebooks|
|Άλλα δείγματα | c:\dsvm\samples|
| Anaconda (προεπιλογή: Python 2.7) | c:\Anaconda |
| Περιβάλλον Python 3.5 anaconda | c:\Anaconda\envs\py35|
|Αυτόνομο διακομιστή R παρουσία καταλόγου (R προεπιλεγμένη παρουσία) | C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| Κατάλογος παρουσία στη βάση δεδομένων διακομιστή R | C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Διάφορα εργαλεία | c:\dsvm\tools|

>[AZURE.NOTE] Παρουσίες των δεδομένων Science εικονική μηχανή της Microsoft δημιουργήθηκαν πριν από την 1.5.0 (πριν από την 3 Σεπτεμβρίου 2016) χρησιμοποιείται μια δομή καταλόγου ελαφρώς διαφορετικό από αυτόν που καθορίζεται στον προηγούμενο πίνακα. 

## <a name="next-steps"></a>Επόμενα βήματα
Εδώ θα βρείτε ορισμένα επόμενα βήματα για να συνεχίσετε την εκμάθηση και Εξερεύνηση. 

* Εξερευνήστε τα διάφορα εργαλεία science δεδομένων σε τα δεδομένα science Εικονική κάνοντας κλικ στο μενού "Έναρξη" και ανάληψη ελέγχου των εργαλείων στη λίστα του μενού.
* Περιήγηση στο **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** δείγματα χρησιμοποιώντας τη βιβλιοθήκη RevoScaleR στο R που υποστηρίζει ανάλυση δεδομένων σε κλίμακα για μεγάλες επιχειρήσεις.  
* Διαβάστε το άρθρο: [10 πράγματα που μπορείτε να κάνετε σε τα δεδομένα science εικονική μηχανή](http://aka.ms/dsvmtenthings)
* Μάθετε πώς να δημιουργείτε τελικών αναλυτικής λύσεις συστηματική χρησιμοποιώντας τη [Διαδικασία Science δεδομένων ομάδας](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Επισκεφθείτε τη [Συλλογή πληροφοριών Cortana](http://gallery.cortanaintelligence.com) για μηχανικής εκμάθησης δεδομένων ανάλυσης δειγμάτων και που χρησιμοποιούν την οικογένεια πληροφοριών Cortana. Επίσης παρέχουμε ένα εικονίδιο στο μενού **Έναρξη** και στην επιφάνεια εργασίας του υπολογιστή εικονικές σε αυτήν τη συλλογή.

