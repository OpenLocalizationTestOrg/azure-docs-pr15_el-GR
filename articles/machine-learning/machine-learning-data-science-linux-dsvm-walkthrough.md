<properties 
    pageTitle="Science δεδομένων σε την Linux δεδομένων Science εικονική μηχανή | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να εκτελούν πολλές κοινές εργασίες science δεδομένων με την εικονική Μηχανή Science δεδομένων Linux." 
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev;paulsh" />


# <a name="data-science-on-the-linux-data-science-virtual-machine"></a>Δεδομένα φυσικής σε την Linux δεδομένων Science εικονική μηχανή

Αυτόν τον οδηγό σας δείχνει πώς να εκτελούν πολλές κοινές εργασίες science δεδομένων με την εικονική Μηχανή Science δεδομένων Linux. Η εικονική μηχανή Science δεδομένων Linux (DSVM) είναι διαθέσιμη στο Azure που είναι προεγκατεστημένα με μια συλλογή εργαλείων που χρησιμοποιούνται ευρέως για ανάλυση δεδομένων και μηχανικής εκμάθησης εικονική μηχανή εικόνα. Τα στοιχεία κλειδιού λογισμικού είναι αναλυτικών στο θέμα [παροχή η εικονική μηχανή Linux δεδομένων επιστήμης](machine-learning-data-science-linux-dsvm-intro.md) . Η εικόνα Εικονική διευκολύνει να επιτύχετε γρήγορα αποτελέσματα κάνοντας science δεδομένων σε λεπτά, χωρίς να χρειάζεται να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους κάθε ένα από τα εργαλεία ξεχωριστά. Μπορείτε εύκολα κλιμάκωσης η Εικονική, εάν είναι απαραίτητο, και να διακόψετε την όταν δεν είναι σε χρήση. Επομένως, αυτός ο πόρος είναι τόσο ελαστικά και οικονομικά. 

Οι εργασίες science δεδομένων επίδειξη σε αναλυτικές οδηγίες για αυτό, ακολουθήστε τα βήματα που περιγράφονται στη [Διαδικασία Science δεδομένων ομάδας](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Αυτή η διαδικασία παρέχει μια συστηματική προσέγγιση επιστήμης δεδομένων που επιτρέπει στις ομάδες δεδομένων επιστημόνων για αποτελεσματική συνεργασία μέσω του κύκλου ζωής της δόμησης Έξυπνες εφαρμογές. Διαδικασίας science δεδομένων παρέχει επίσης ένα πλαίσιο επαναληπτικού για science δεδομένων που μπορεί να ακολουθείται από ένα συγκεκριμένο άτομο.

Θα σας ανάλυση του συνόλου δεδομένων [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) σε αυτές τις οδηγίες. Αυτό είναι ένα σύνολο μηνυμάτων ηλεκτρονικού ταχυδρομείου που έχουν επισημανθεί ως ανεπιθύμητη αλληλογραφία ή χοιρομέρι (δηλαδή, δεν είναι ανεπιθύμητη αλληλογραφία), και περιλαμβάνει επίσης ορισμένες στατιστικά στοιχεία σχετικά με το περιεχόμενο από τα μηνύματα ηλεκτρονικού ταχυδρομείου. Τα στατιστικά στοιχεία που περιλαμβάνονται περιγράφονται στην επόμενη, αλλά μία ενότητα. 


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Μπορείτε να χρησιμοποιήσετε μια εικονική μηχανή Linux δεδομένων φυσικής, πρέπει να έχετε τα εξής:

- Μια **συνδρομή Azure**. Εάν δεν έχετε ήδη ένα, ανατρέξτε στο θέμα [Δημιουργία σήμερα το δωρεάν λογαριασμό Azure](https://azure.microsoft.com/free/).
- Μια [**Linux δεδομένων science Εικονική**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Για πληροφορίες σχετικά με την προμήθεια Εικονική αυτό, ανατρέξτε στο θέμα [παροχή η εικονική μηχανή Linux δεδομένων επιστήμης](machine-learning-data-science-linux-dsvm-intro.md). 
- [X2Go](http://wiki.x2go.org/doku.php) εγκατεστημένο στον υπολογιστή σας και ανοίγει μια περίοδο λειτουργίας XFCE. Για πληροφορίες σχετικά με την εγκατάσταση και τη ρύθμιση των παραμέτρων ενός **προγράμματος-πελάτη X2Go**, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων X2Go προγράμματος-πελάτη](machine-learning-data-science-linux-dsvm-intro.md#Installing-and-configuring-X2Go-client). 
- Ένας **λογαριασμός AzureML**. Εάν δεν έχετε ήδη ένα, εγγραφείτε για καινούργιο κατά την [αρχική σελίδα AzureML](https://studio.azureml.net/). Υπάρχει μια δωρεάν χρήση επιπέδων για να σας βοηθήσει να ξεκινήσετε.


## <a name="download-the-spambase-dataset"></a>Κάντε λήψη του συνόλου δεδομένων spambase

Το σύνολο δεδομένων [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) είναι ένα σχετικά μικρό σύνολο των δεδομένων που περιέχει μόνο 4601 παραδείγματα. Αυτό είναι ένα εύχρηστο μέγεθος για χρήση κατά την επίδειξη ότι ορισμένες από τις βασικές δυνατότητες του η Εικονική Science δεδομένων όπως διατηρεί τις απαιτήσεις πόρων μέτρια.

>[AZURE.NOTE] Αυτή η αναλυτική παρουσίαση δημιουργήθηκε σε μια D2 μεγέθους v2 Linux δεδομένων Science εικονική μηχανή. Αυτό το μέγεθος DSVM είναι δυνατό να χειρίζεται τις διαδικασίες σε αυτές τις οδηγίες.

Εάν χρειάζεστε περισσότερο χώρο αποθήκευσης, μπορείτε να δημιουργήσετε πρόσθετες δίσκων και να επισυνάψετε σε Εικονική σας. Αυτών των δίσκων χρησιμοποιεί μόνιμη Azure χώρο αποθήκευσης, έτσι ώστε τα δεδομένα διατηρούνται ακόμα και όταν ο διακομιστής είναι reprovisioned λόγω αλλαγή μεγέθους ή τερματίζεται. Για να προσθέσετε ένα δίσκο και να το επισυνάψετε Εικονική σας, ακολουθήστε τις οδηγίες στο θέμα [Προσθήκη ένα δίσκο σε μια Εικονική Linux](../virtual-machines/virtual-machines-linux-add-disk.md). Αυτά τα βήματα χρησιμοποιούν το περιβάλλον γραμμής εντολών Azure (Azure CLI), που είναι ήδη εγκατεστημένος στον το DSVM. Επομένως, μπορεί να γίνει αυτές τις διαδικασίες εντελώς από το Εικονική ίδια. Μια άλλη επιλογή για να αυξήσετε το χώρο αποθήκευσης είναι να χρησιμοποιήσετε [Azure αρχεία](../storage/storage-how-to-use-files-linux.md).

Για να κάνετε λήψη των δεδομένων, ανοίξτε ένα παράθυρο τερματικού και εκτέλεση αυτής της εντολής:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

Το αρχείο που έχετε λάβει δεν έχει γραμμή επικεφαλίδας, οπότε ας δημιουργήσετε άλλο αρχείο που έχετε μια κεφαλίδα. Εκτέλεση αυτής της εντολής για να δημιουργήσετε ένα αρχείο με τις κατάλληλες κεφαλίδες:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Στη συνέχεια, να συνενώσετε δύο αρχεία μαζί με την εντολή:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

Το σύνολο δεδομένων έχει πολλούς τύπους στατιστικά στοιχεία σε κάθε μήνυμα ηλεκτρονικού ταχυδρομείου: 

- Στήλες όπως ***word\_freq\_WORD*** υποδεικνύουν το ποσοστό των λέξεων σε μήνυμα ηλεκτρονικού ταχυδρομείου που ταιριάζουν με *το WORD*. Για παράδειγμα, εάν *word\_freq\_κάνετε* είναι 1, τότε 1% όλων των λέξεων στο μήνυμα ηλεκτρονικού ταχυδρομείου έχουν *κάνετε*. 
- Στήλες όπως ***char\_freq\_CHAR*** υποδεικνύουν το ποσοστό όλων των χαρακτήρων σε μήνυμα ηλεκτρονικού ταχυδρομείου που έχουν *ένα ΧΑΡΑΚΤΉΡΑ*. 
- ***κεφαλαίο\_εκτέλεση\_μήκος\_μεγαλύτερου*** είναι το μήκος του μεγαλύτερου από μια ακολουθία κεφαλαία γράμματα. 
- ***κεφαλαίο\_εκτέλεση\_μήκος\_μέσος όρος*** είναι το μέσο μήκος της ακολουθίες όλα κεφαλαία γράμματα. 
- ***κεφαλαίο\_εκτέλεση\_μήκος\_συνολικό*** είναι του συνολικού μήκους της ακολουθίες όλα κεφαλαία γράμματα. 
- ***Ανεπιθύμητη αλληλογραφία*** υποδεικνύει αν το μήνυμα ηλεκτρονικού ταχυδρομείου θεωρήθηκε ανεπιθύμητης αλληλογραφίας ή όχι (1 = ανεπιθύμητης αλληλογραφίας, 0 = μη ανεπιθύμητη αλληλογραφία).


## <a name="explore-the-dataset-with-microsoft-r-open"></a>Εξερεύνηση του συνόλου δεδομένων με το Microsoft R ανοιχτό

Εξετάστε τα δεδομένα και να κάνετε ορισμένες βασικές μηχανικής εκμάθησης με R. Ας Η Εικονική Science δεδομένων συνοδεύεται από [Microsoft R Άνοιγμα](https://mran.revolutionanalytics.com/open/) προ-εγκατεστημένο. Οι βιβλιοθήκες πολυνηματική μαθηματικές πράξεις σε αυτήν την έκδοση του R προσφέρουν καλύτερες επιδόσεις από διάφορες εκδόσεις μονού νήματος. Άνοιγμα R Microsoft παρέχει επίσης αναπαραγωγιμότητας, χρησιμοποιώντας ένα στιγμιότυπο του αποθετηρίου πακέτου CRAN.

Για να λάβετε αντίγραφα των τα δείγματα κώδικα που χρησιμοποιούνται σε αυτές τις οδηγίες, κλωνοποίηση του αποθετηρίου **Azure-μηχανικής-εκμάθησης-δεδομένων-Science** χρησιμοποιώντας git, που είναι προεγκατεστημένα στον η Εικονική. Από τη γραμμή εντολών git, εκτελέστε:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Ανοίξτε ένα παράθυρο τερματικού και έναρξη μιας νέας περιόδου λειτουργίας R με την κονσόλα αλληλεπιδραστικών R.

>[AZURE.NOTE] Μπορείτε επίσης να χρησιμοποιήσετε RStudio για τις παρακάτω διαδικασίες. Για να εγκαταστήσετε RStudio, εκτέλεση αυτής της εντολής σε ένα terminal:`./Desktop/DSVM\ tools/installRStudio.sh`

Για να εισαγάγετε τα δεδομένα και να ρυθμίσετε το περιβάλλον, εκτελέστε:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Για να δείτε συνοπτικές στατιστικά στοιχεία σχετικά με κάθε στήλη:

    summary(data)

Για μια διαφορετική προβολή των δεδομένων:

    str(data)

Αυτή παρουσιάζονται τον τύπο της κάθε μεταβλητή και την πρώτη μερικές τιμές στο του συνόλου δεδομένων. 

Στήλη *ανεπιθύμητης αλληλογραφίας* αναγνώστηκε μορφή ακέραιου αριθμού, αλλά είναι στην πραγματικότητα μια έλαβε μεταβλητή (ή παραγόντων). Για να ορίσετε τον τύπο:

    data$spam <- as.factor(data$spam)

Για να κάνετε ορισμένες διερευνητικές ανάλυση, χρησιμοποιήστε το πακέτο [ggplot2](http://ggplot2.org/) , μια βιβλιοθήκη δημοφιλείς γραφική για R που είναι ήδη εγκατεστημένος στον η Εικονική. Σημειώστε ότι από τα δεδομένα σύνοψης που εμφανίστηκε προηγουμένως, έχουμε σύνοψης στατιστικά στοιχεία σχετικά με τη συχνότητα του χαρακτήρα θαυμαστικό. Ας απεικονίσετε αυτά τα συχνότητας με τις παρακάτω εντολές:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Επειδή το μηδέν γραμμή είναι αλλοιώνοντας τα σχεδίασης, ας την εξαφανίσουμε:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Υπάρχει μια μη-μικρής σημασίας πυκνότητας παραπάνω 1 που έχει ενδιαφέρον. Ας ρίξουμε μια ματιά μόνο αυτά τα δεδομένα:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Στη συνέχεια, τη διαιρέσετε από ανεπιθύμητη αλληλογραφία και στο ham:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Αυτά τα παραδείγματα, θα πρέπει να σάς επιτρέπουν να κάνετε παρόμοια σχεδιάσεων των άλλων στηλών για να εξερευνήσετε τα δεδομένα που περιέχονται σε αυτές.


## <a name="train-and-test-an-ml-model"></a>Εκπαίδευση και δοκιμάστε ένα μοντέλο ML

Τώρα ας εκπαίδευση μερικά μηχανικής εκμάθησης μοντέλων για να ταξινομήσετε τα μηνύματα ηλεκτρονικού ταχυδρομείου στο του συνόλου δεδομένων ως που περιέχει διάστημα ή ham. Προσπαθούμε να εκπαιδεύσετε μοντέλου δέντρου αποφάσεων και σε ένα μοντέλο τυχαία δάσος σε αυτήν την ενότητα και, στη συνέχεια, να ελέγξετε την ακρίβεια των προβλέψεων τους. 

>[AZURE.NOTE] Το πακέτο rpart (επαναλαμβανόμενες διαμερισμάτων και δέντρα παλινδρόμησης) χρησιμοποιείται τον παρακάτω κώδικα είναι ήδη εγκατεστημένο στον η Εικονική Science δεδομένων.


Πρώτα, ας διαίρεση του συνόλου δεδομένων σε σύνολα εκπαίδευση και δοκιμής:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

Και, στη συνέχεια, δημιουργία δέντρου αποφάσεων για να ταξινομήσετε τα μηνύματα ηλεκτρονικού ταχυδρομείου.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Αυτό είναι το αποτέλεσμα:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

Για να καθορίσετε πόσο καλά εκτελέσει στο σύνολο εκπαίδευση, χρησιμοποιήστε τον ακόλουθο κώδικα:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Για να καθορίσετε πόσο καλά εκτελέσει στο σύνολο δοκιμής:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Ας δοκιμάσουμε επίσης ένα μοντέλο τυχαία δάσος. Τυχαίες συμπλεγμάτων δομών εκπαίδευση ένα πλήθος δέντρα αποφάσεων και εξόδου μιας κλάσης που είναι η λειτουργία του ταξινομήσεων από όλα τα δέντρα μεμονωμένα απόφασης. Παρέχουν μια πιο ισχυρή μηχανικής εκμάθησης προσέγγιση, όπως αυτά διόρθωση για την τάση ενός μοντέλου δέντρου αποφάσεων να overfit ένα σύνολο δεδομένων εκπαίδευσης. 

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Ανάπτυξη ενός μοντέλου Azure ML

[Azure μηχανικής εκμάθησης Studio](https://studio.azureml.net/) (AzureML) είναι μια υπηρεσία cloud που διευκολύνει την Δημιουργήστε και αναπτύξτε μοντέλα πρόβλεψης ανάλυσης. Μία από τις δυνατότητες όμορφες και του AzureML είναι η δυνατότητα να δημοσιεύσετε οποιαδήποτε συνάρτηση R ως υπηρεσία web. Το πακέτο AzureML R εύκολα ανάπτυξης για να το κάνετε απευθείας από μας περίοδο λειτουργίας R στον το DSVM. 

Για να αναπτύξετε τον κώδικα δέντρου αποφάσεων από την προηγούμενη ενότητα, πρέπει να συνδεθείτε στο Azure μηχανικής εκμάθησης Studio. Χρειάζεστε το Αναγνωριστικό του χώρου εργασίας και ένα διακριτικό εξουσιοδότηση να sigh στο. Για να βρείτε αυτές τις τιμές και προετοιμασία των μεταβλητών AzureML μαζί τους:

Επιλέξτε **Ρυθμίσεις** στο αριστερό μενού. Σημειώστε το **Αναγνωριστικό χώρου ΕΡΓΑΣΊΑΣ**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

Επιλέξτε **Εξουσιοδότηση διακριτικά** από το μενού επιβάρυνσης και σημειώστε την **Κύρια εξουσιοδότησης διακριτικού**. ![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

Φόρτωση του πακέτου **AzureML** και, στη συνέχεια, ρύθμιση τιμές των μεταβλητών με το Αναγνωριστικό διακριτικού και χώρου εργασίας στην περίοδο λειτουργίας σας R σε το DSVM:


    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Ας απλοποιήσετε το μοντέλο για να διευκολύνετε αυτήν την επίδειξη για την υλοποίηση. Επιλέξτε τις τρεις μεταβλητές στο πλησιέστερο στον ριζικό κατάλογο του δέντρου αποφάσεων και δημιουργήστε μια νέα δομή χρησιμοποιώντας απλώς αυτές τις τρεις μεταβλητές:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Χρειαζόμαστε μια συνάρτηση πρόβλεψης που λαμβάνει τις δυνατότητες ως εισαγωγή και επιστρέφει τις προβλεπόμενες τιμές:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

Δημοσίευση της συνάρτησης predictSpam AzureML με χρήση της συνάρτησης **publishWebService** : 

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Αυτή η συνάρτηση διαρκεί η συνάρτηση **predictSpam** δημιουργεί μια υπηρεσία web με το όνομα **spamWebService** με καθορισμένο εισροές και εκροές και επιστρέφει πληροφορίες για το νέο τελικό σημείο.

Προβάλετε τις λεπτομέρειες της υπηρεσίας web που έχει δημοσιευθεί, συμπεριλαμβανομένων τελικό σημείο του API και αποκτήστε πρόσβαση στο αριθμών-κλειδιών με την εντολή:

    spamWebService[[2]]

Για να το δοκιμάσετε στην πρώτη ρύθμιση 10 γραμμές της δοκιμής:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Χρήση άλλων εργαλείων διαθέσιμη

Οι υπόλοιπες ενότητες δείχνουν πώς μπορείτε να χρησιμοποιήσετε ορισμένα από τα εργαλεία εγκατεστημένα στον η Εικονική Linux δεδομένων επιστήμης. Ακολουθεί η λίστα με τα εργαλεία αναφέρονται:

- XGBoost
- Python
- Jupyterhub
- Rattle
- PostgreSQL & σκίουρου SQL
- Αποθήκη δεδομένων του SQL Server


## <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) είναι ένα εργαλείο που παρέχει μια εφαρμογή γρήγορα και ακριβή ενισχύεται δέντρου.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost επίσης να καλέσετε από python ή μια γραμμή εντολών.

## <a name="python"></a>Python

Για την ανάπτυξη χρησιμοποιώντας Python, το κατανομές Anaconda Python 2.7 και 3.5 έχουν εγκατασταθεί στο το DSVM. 

>[AZURE.NOTE] Η κατανομή Anaconda περιλαμβάνει [Condas](http://conda.pydata.org/docs/index.html), που μπορεί να χρησιμοποιηθεί για να δημιουργήσετε προσαρμοσμένα περιβάλλοντα για Python που έχουν διαφορετικές εκδόσεις ή/και πακέτα που είναι εγκατεστημένα στο τους.

Ας ανάγνωση σε ορισμένες από το σύνολο δεδομένων spambase και να ταξινομείτε τα μηνύματα ηλεκτρονικού ταχυδρομείου με μηχανές ανύσματος υποστήριξης στο scikit-μάθετε:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Για να κάνετε προβλέψεις:

    clf.predict(X.ix[0:20, :])

Για να εμφανίσετε τον τρόπο για να δημοσιεύσετε ένα τελικό σημείο AzureML, ας κάνουμε μοντέλου απλούστερο τις τρεις μεταβλητές όπως κάναμε όταν που δημοσιεύτηκε το μοντέλο R προηγουμένως. 

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Για να δημοσιεύσετε το μοντέλο να AzureML:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

>[AZURE.NOTE] Αυτή είναι διαθέσιμη μόνο για python 2.7 και δεν υποστηρίζεται ακόμη σε 3.5. Εκτέλεση με **/anaconda/bin/python2.7**.


## <a name="jupyterhub"></a>Jupyterhub

Η κατανομή Anaconda στο το DSVM συνοδεύεται από ένα σημειωματάριο Jupyter, ένα περιβάλλον πλατφόρμα για την κοινή χρήση Python, R ή Julia κώδικα και ανάλυση. Το Σημειωματάριο Jupyter γίνεται μέσω JupyterHub. Έχετε εισέλθει με το τοπικό όνομα χρήστη Linux και τον κωδικό πρόσβασης στο ***https://\<Εικονική DNS όνομα ή τη διεύθυνση IP\>: 8000 /***. Όλα τα αρχεία ρύθμισης παραμέτρων για JupyterHub βρίσκονται στον κατάλογο **/etc/jupyterhub**.

Πολλά σημειωματάρια του δείγματος είναι ήδη εγκατεστημένα στον η Εικονική:

- Ανατρέξτε στο θέμα το [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) για ένα σημειωματάριο Python δείγμα.
- Ανατρέξτε στο θέμα [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) για ένα σημειωματάριο **R** δείγμα.
- Ανατρέξτε στο θέμα το [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) για ένα άλλο σημειωματάριο **Python** δείγμα.

>[AZURE.NOTE] Η γλώσσα Julia είναι επίσης διαθέσιμη από τη γραμμή εντολών στον η Εικονική Science δεδομένων Linux.


## <a name="rattle"></a>Rattle

[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (το R αναλυτικής εργαλείο για να μάθετε εύκολα) είναι ένα εργαλείο γραφικών R για εξόρυξης δεδομένων. Έχει ένα διαισθητικό περιβάλλον εργασίας που διευκολύνει την φόρτωση, εξερευνήστε, και μετασχηματισμού δεδομένων και δημιουργία και αξιολόγηση μοντέλων.  Το άρθρο [Rattle: A δεδομένων εξορυκτικού Γραφικών για R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) παρέχει αναλυτικές οδηγίες που δείχνει τις δυνατότητές του.

Εγκαταστήστε και ξεκινήστε Rattle με τις παρακάτω εντολές:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

>[AZURE.NOTE] Δεν απαιτείται εγκατάσταση σε το DSVM. Αλλά Rattle μπορεί να σας ζητήσει να εγκαταστήσετε πρόσθετα πακέτα όταν φορτώνεται.

Rattle χρησιμοποιεί ένα περιβάλλον που βασίζεται σε καρτέλα. Οι περισσότερες από τις καρτέλες αντιστοιχούν στα βήματα της [Διαδικασίας Science δεδομένων](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), όπως γίνεται φόρτωση των δεδομένων ή την Εξερεύνηση. Η διαδικασία science δεδομένων ρέει από αριστερά προς τα δεξιά ανάμεσα στις καρτέλες. Αλλά τελευταίας καρτέλας περιέχει ένα αρχείο καταγραφής από τις εντολές R εκτέλεση με Rattle. 


Για να φορτώσετε και να ρυθμίσετε τις παραμέτρους του συνόλου δεδομένων:

- Για να φορτώσετε το αρχείο, επιλέξτε την καρτέλα **δεδομένα** και, στη συνέχεια 
- Επιλέξτε το κουμπί επιλογής δίπλα στο **όνομα αρχείου** και επιλέξτε **spambaseHeaders.data**. 
- Για να φορτώσετε το αρχείο. Επιλέξτε **Εκτέλεση** στην πρώτη γραμμή των κουμπιών. Θα πρέπει να μπορείτε να δείτε μια σύνοψη των κάθε στήλη, όπως τον τύπο δεδομένων προσδιορισμένο, είτε πρόκειται για μια εισαγωγή δεδομένων, ο στόχος ή άλλου τύπου μεταβλητή, καθώς και τον αριθμό των μοναδικών τιμών.
- Rattle έχει εντοπίσει σωστά τη στήλη **ανεπιθύμητης ηλεκτρονικής αλληλογραφίας** ως προορισμό. Επιλέξτε τη στήλη ανεπιθύμητης αλληλογραφίας και, στη συνέχεια, ορίστε τον **Τύπο δεδομένων προορισμού** σε **Categoric**.

Για να εξερευνήσετε τα δεδομένα: 

- Επιλέξτε την καρτέλα **Εξερεύνηση** . 
- Κάντε κλικ στην επιλογή **σύνοψης**, στη συνέχεια, **Εκτέλεση**, για να δείτε ορισμένες πληροφορίες σχετικά με τους τύπους μεταβλητών και ορισμένες σύνοψης στατιστικά στοιχεία. 
- Για να προβάλετε άλλους τύπους στατιστικά στοιχεία σχετικά με κάθε μεταβλητή, ενεργοποιήστε άλλες επιλογές όπως **περιγράφουν** ή **βασικές δυνατότητες**.

Στην καρτέλα **Εξερεύνηση** σας επιτρέπει επίσης να δημιουργήσετε πολλές Έξυπνες σχεδιάσεων. Για να σχεδιάσετε ένα ιστόγραμμα των δεδομένων:


- Επιλέξτε **κατανομές**.
- Δείτε **ιστόγραμμα** για **word_freq_remove** και **word_freq_you**.
- Επιλέξτε **Εκτέλεση**. Θα πρέπει να βλέπετε δύο σχεδιάσεων πυκνότητας σε ένα μεμονωμένο graph παράθυρο, όπου είναι σαφές ότι η λέξη "που" εμφανίζεται πολύ πιο συχνά σε μηνύματα ηλεκτρονικού ταχυδρομείου από "Κατάργηση".

Το διάγραμμα συσχέτισης είναι επίσης ενδιαφέρον. Για να δημιουργήσετε ένα:


- Στη συνέχεια, επιλέξτε **συσχέτισης** ως τον **τύπο**, 
- Επιλέξτε **Εκτέλεση**. 
- Rattle εμφανίζει μια προειδοποίηση που προτείνει έως 40 μεταβλητές. Επιλέξτε **Ναι** για να προβάλετε το σχεδίασης. 

Υπάρχουν ορισμένα ενδιαφέρον συσχετίσεων που προκύπτουν: "η τεχνολογία" είναι ιδιαίτερα συσχετιστούν με "HP" και "labs", για παράδειγμα. Το επίσης συνιστάται ιδιαίτερα είναι συσχετισμένη με "650", επειδή ο κωδικός περιοχής από το σύνολο δεδομένων χορηγούς είναι 650.

Οι αριθμητικές τιμές για τους συσχετισμούς μεταξύ των λέξεων είναι διαθέσιμες στο παράθυρο "Εξερεύνηση". Είναι ενδιαφέρον για σημείωση, για παράδειγμα, ότι "η τεχνολογία" αρνητικά είναι συσχετισμένη με "σας" και "χρήματα".

Rattle να μετατρέψετε το σύνολο δεδομένων για το χειρισμό ορισμένα κοινά θέματα. Για παράδειγμα, σάς επιτρέπει να Επανάληψη κλιμάκωσης δυνατότητες, τεκμαίρει τιμές που λείπουν, χειριστείτε ακραίες τιμές και κατάργηση μεταβλητές ή παρατηρήσεις με δεδομένα που λείπουν. Rattle επίσης να αναγνωρίσετε κανόνες συσχέτισης μεταξύ παρατηρήσεις ή/και μεταβλητές. Αυτές οι καρτέλες είναι εκτός εμβέλειας για αυτόν τον Οδηγό εισαγωγικό.

Rattle επίσης να εκτελέσετε ανάλυση σύμπλεγμα. Ας εξαίρεση ορισμένες δυνατότητες που μπορείτε να κάνετε πιο ευανάγνωστο το αποτέλεσμα. Στην καρτέλα **δεδομένα** , επιλέξτε **Παράβλεψη** δίπλα σε κάθε μία από τις μεταβλητές εκτός από αυτές τις δέκα στοιχείων:

- word_freq_hp
- word_freq_technology
- word_freq_george
- word_freq_remove
- word_freq_your
- word_freq_dollar
- word_freq_money
- capital_run_length_longest
- word_freq_business
- ανεπιθύμητη αλληλογραφία

Στη συνέχεια, μεταβείτε στην καρτέλα **σύμπλεγμα** , επιλέξτε **KMeans**και ορίστε τον *αριθμό των συμπλεγμάτων* σε 4. Στη συνέχεια, η **Εκτέλεση**. Τα αποτελέσματα εμφανίζονται στο παράθυρο εξόδου. Ένα σύμπλεγμα έχει υψηλή συχνότητα "Γιώργος" και "hp" και είναι πιθανώς ένα νόμιμο επαγγελματικό ηλεκτρονικό ταχυδρομείο.

Για να δημιουργήσετε ένα μοντέλο απλό απόφασης δέντρου μηχανικής εκμάθησης: 

- Επιλέξτε την καρτέλα **μοντέλο** , 
- Επιλέξτε **δέντρου** ως τον **τύπο**. 
- Επιλέξτε **Εκτέλεση** για να εμφανίσετε το δέντρο σε μορφή κειμένου στο παράθυρο εξόδου. 
- Επιλέξτε το κουμπί **Σχεδίαση** για να προβάλετε μια έκδοση γραφικών. Απολύτως αυτή μοιάζει με το δέντρο μας που λαμβάνονται με προηγούμενη έκδοση *rpart*.

Μία από τις δυνατότητες όμορφες και του Rattle είναι η δυνατότητα για να εκτελέσετε διάφορες μεθόδους εκμάθησης υπολογιστή και γρήγορο υπολογισμό τους. Ακολουθεί η διαδικασία:

- Επιλέξτε **όλα** για τον **τύπο**. 
- Επιλέξτε **Εκτέλεση**. 
- Μετά την ολοκλήρωση της μπορείτε να επιλέξετε οποιονδήποτε μεμονωμένο **τύπο**, όπως **SVM**, και να προβάλετε τα αποτελέσματα. 
- Μπορείτε επίσης να συγκρίνετε τις επιδόσεις της τα υποδείγματα κατά την επικύρωση ρύθμιση χρησιμοποιώντας την καρτέλα **αξιολόγηση** . Για παράδειγμα, στην επιλογή **Πίνακας σφάλματος** εμφανίζεται η μήτρα σύγχυση, συνολική σφάλμα και μέσων κλάσης σφάλματος για κάθε μοντέλο το σύνολο επικύρωσης. 
- Μπορείτε επίσης να απεικονίσετε ROC καμπύλες, εκτελέσετε ανάλυση ευαισθησίας και κάντε άλλους τύπους αξιολογήσεις μοντέλο.

Αφού ολοκληρώσετε τη δημιουργία μοντέλων, επιλέξτε την καρτέλα **αρχείο καταγραφής** για να προβάλετε τον κώδικα R εκτέλεση με Rattle κατά την περίοδο λειτουργίας σας. Μπορείτε να επιλέξετε το κουμπί **εξαγωγής** για να το αποθηκεύσετε. 

>[AZURE.NOTE] Υπάρχει ένα σφάλμα στην τρέχουσα έκδοση της Rattle. Για να τροποποιήσετε τη δέσμη ενεργειών ή χρησιμοποιήστε το για να επαναλάβετε τα βήματα αργότερα, που πρέπει να εισαγάγετε έναν χαρακτήρα # μπροστά από *εξαγωγή... Αυτό το αρχείο καταγραφής* στο κείμενο του αρχείου καταγραφής. 


## <a name="postgresql--squirrel-sql"></a>PostgreSQL & σκίουρου SQL

Το DSVM συνοδεύεται από PostgreSQL εγκατεστημένο. PostgreSQL είναι ένα εξελιγμένο, Άνοιγμα προέλευσης σχεσιακή βάση δεδομένων. Αυτή η ενότητα δείχνει πώς μπορείτε να φορτώσετε το σύνολο δεδομένων ανεπιθύμητης αλληλογραφίας στο PostgreSQL και, στη συνέχεια, το ερώτημα.

Μπορείτε να φορτώσετε τα δεδομένα, πρέπει να επιτρέπει έλεγχο ταυτότητας κωδικού πρόσβασης από το τοπικού κεντρικού υπολογιστή. Σε μια γραμμή εντολών:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Κοντά στο κάτω μέρος του αρχείου ρύθμισης παραμέτρων είναι περισσότερες από μία γραμμές που το επιτρεπόμενων συνδέσεων με λεπτομέρειες:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Αλλάξτε τη γραμμή "Συνδέσεις τοπικού IPv4" για να χρησιμοποιήσετε md5 αντί για ident, ώστε να σας μπορεί να συνδεθείτε χρησιμοποιώντας ένα όνομα χρήστη και τον κωδικό πρόσβασης:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

Και επανεκκινήστε την υπηρεσία postgres:

    sudo systemctl restart postgresql

Για να εμφανίσετε psql, μια terminal αλληλεπίδραση για PostgreSQL, ως το ενσωματωμένο postgres χρήστη, εκτελέστε την ακόλουθη εντολή από ένα μήνυμα:

    sudo -u postgres psql

Δημιουργία νέου λογαριασμού χρήστη, χρησιμοποιώντας το ίδιο όνομα χρήστη με το λογαριασμό Linux έχετε συνδεθεί αυτήν τη στιγμή ως, και να της δώσετε έναν κωδικό πρόσβασης:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Στη συνέχεια, συνδεθείτε στο psql ως χρήστη σας:

    psql

Και εισαγάγετε τα δεδομένα σε μια νέα βάση δεδομένων:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Τώρα, ας εξετάσουμε τα δεδομένα και να εκτελείτε ορισμένες ερωτημάτων με **Σκίουρου SQL**, ένα εργαλείο γραφικών που σας επιτρέπει να αλληλεπιδράτε με βάσεις δεδομένων μέσω ενός προγράμματος οδήγησης JDBC.

Για να ξεκινήσετε, εκκίνηση σκίουρου SQL από το μενού "εφαρμογές". Για να ρυθμίσετε το πρόγραμμα οδήγησης:

- Επιλογή **των Windows**, στη συνέχεια, **προγράμματα οδήγησης προβολή**. 
- Κάντε δεξί κλικ στο **PostgreSQL** και επιλέξτε **Το πρόγραμμα οδήγησης τροποποίηση**. 
- Επιλέξτε **διαδρομής επιπλέον κλάσεων**, στη συνέχεια, **προσθέσετε**. 
- Εισαγάγετε ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** για το **όνομα του αρχείου** και 
- Επιλέξτε **Άνοιγμα**.
- Λίστα προγραμμάτων οδήγησης, επιλέξτε, στη συνέχεια, επιλέξτε **org.postgresql.Driver** στο πλαίσιο **Όνομα κατηγορίας**, και επιλέξτε **OK.**

Για να ρυθμίσετε τη σύνδεση με τον τοπικό διακομιστή:
 
- Στη συνέχεια, επιλέξτε **Windows** **Προβολή ψευδώνυμα.** 
- Επιλέξτε το **+** κουμπί για να κάνετε ένα νέο ψευδώνυμο. 
- Ονομάστε το *βάση δεδομένων της ανεπιθύμητης αλληλογραφίας*, επιλέξτε **PostgreSQL** στο **πρόγραμμα οδήγησης το** αναπτυσσόμενο μενού.
- Ορισμός της διεύθυνσης URL σε *jdbc:postgresql://localhost/spam*. 
- Πληκτρολογήστε το *όνομα χρήστη* και *τον κωδικό πρόσβασης*. 
- Κάντε κλικ στο **κουμπί OK**. 
- Για να ανοίξετε το παράθυρο **σύνδεσης** , κάντε διπλό κλικ το ψευδώνυμο ***βάση δεδομένων της ανεπιθύμητης αλληλογραφίας*** . 
- Επιλέξτε **σύνδεση**.

Για να εκτελέσετε ορισμένες ερωτημάτων:

- Επιλέξτε την καρτέλα **SQL** .
- Εισαγάγετε ένα απλό ερώτημα, όπως `SELECT * from data;` στο πλαίσιο κειμένου του ερωτήματος στο επάνω μέρος της καρτέλας SQL. 
- Πατήστε το **Συνδυασμό πλήκτρων Ctrl-Enter** για να το εκτελέσετε. Από προεπιλογή σκίουρου SQL επιστρέφει των πρώτων 100 γραμμών από το ερώτημά σας. 

Υπάρχουν πολλά περισσότερα ερωτήματα που θα μπορούσατε να εκτελέσετε για να εξερευνήσετε τα δεδομένα. Για παράδειγμα, πώς τη συχνότητα από το word *να* διαφέρουν μεταξύ ανεπιθύμητης αλληλογραφίας και ham;

    SELECT avg(word_freq_make), spam from data group by spam;

Ή τι είναι τα χαρακτηριστικά του ηλεκτρονικού ταχυδρομείου που περιέχουν συχνά *3Δ*;

    SELECT * from data order by word_freq_3d desc;

Τα περισσότερα μηνύματα ηλεκτρονικού ταχυδρομείου που έχουν μια υψηλή εμφάνιση της *3Δ* είναι προφανώς ανεπιθύμητης αλληλογραφίας, ώστε να μπορεί να είναι μια χρήσιμη δυνατότητα για τη δημιουργία ενός μοντέλου πρόβλεψης για να ταξινομήσετε τα μηνύματα ηλεκτρονικού ταχυδρομείου.

Εάν θέλετε να εκτελέσετε μηχανικής εκμάθησης με δεδομένα που είναι αποθηκευμένα σε μια βάση δεδομένων PostgreSQL, εξετάστε το ενδεχόμενο χρήσης [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>Αποθήκη δεδομένων του SQL Server

Αποθήκη δεδομένων του SQL Azure είναι μια βασισμένη στο cloud, κλιμάκωσης βάση δεδομένων με δυνατότητα επεξεργασίας μεγάλους όγκους δεδομένων, τόσο σχεσιακές και μη σχεσιακών. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι η αποθήκη δεδομένων του SQL Azure;](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Για να συνδεθείτε με την αποθήκη δεδομένων και να δημιουργήσετε τον πίνακα, εκτελέστε την ακόλουθη εντολή από μια γραμμή εντολών:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Στη συνέχεια, στη γραμμή εντολών sqlcmd:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Αντιγράψτε τα δεδομένα με bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

>[AZURE.NOTE] Τέλους γραμμής στο αρχείο που έχετε λάβει είναι στυλ των Windows, αλλά bcp αναμένει τύπου UNIX, ώστε να πρέπει να παρέχουμε bcp που με τη σημαία - r.

Και ερώτημα με sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

Μπορείτε επίσης μπορεί να υποβάλετε ερώτημα με σκίουρου SQL. Ακολουθήστε παρόμοια βήματα για PostgreSQL, χρησιμοποιώντας το Microsoft MSSQL JDBC πρόγραμμα οδήγησης Server, όπου μπορείτε να βρείτε στο ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Επόμενα βήματα

Για μια επισκόπηση των θεμάτων που θα σας καθοδηγήσουν για τις εργασίες που περιλαμβάνουν τη διαδικασία Science δεδομένων στο Azure, ανατρέξτε στο θέμα [Διαδικασία Science δεδομένων ομάδας](http://aka.ms/datascienceprocess).

Για μια περιγραφή των άλλων αναλυτικές παρουσιάσεις σε ολοκληρωμένες που δείχνουν τα βήματα της διαδικασίας ομάδας δεδομένων επιστήμης για συγκεκριμένα σενάρια, ανατρέξτε στο θέμα [αναλυτικές διαδικασία Science δεδομένων ομάδας](data-science-process-walkthroughs.md). Αναλυτικές παρουσιάσεις απεικόνιση επίσης πώς μπορείτε να συνδυάσετε εργαλεία cloud και εσωτερικής εγκατάστασης και υπηρεσίες σε μια ροή εργασίας ή τη διαδικασία για να δημιουργήσετε μια έξυπνη εφαρμογή.

