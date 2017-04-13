<properties 
    pageTitle="Λεξικό βάσει ανάλυσης άποψη | Microsoft Azure" 
    description="Λεξικό βάσει άποψη ανάλυσης" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Λεξικό βάσει άποψη ανάλυσης 

Πώς να κάνετε μέτρηση απόψεις των χρηστών και τη στάση προς σήματα ή τα θέματα στην ηλεκτρονική κοινωνικά δίκτυα, όπως δημοσιεύσει Facebook, tweets, αναθεωρήσεις, κ.λπ.; Άποψη ανάλυσης παρέχει μια μέθοδο για την ανάλυση όπως ερωτήσεις.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Γενικά, υπάρχουν δύο μέθοδοι για την ανάλυση άποψη. Μία χρησιμοποιεί έναν αλγόριθμο επιβλεπόμενες εκμάθησης και το άλλο μπορούν να θεωρηθούν λειτουργεί χωρίς επιτήρηση εκμάθησης. Ένας αλγόριθμος επιβλεπόμενες εκμάθησης γενικά δημιουργεί ένα μοντέλο κατάταξης σε μια μεγάλη σώματος με σχόλια. Την ακρίβεια είναι κυρίως με βάση την ποιότητα του σχολίου και συνήθως η διαδικασία εκπαίδευση θα διαρκέσει μεγάλο χρονικό διάστημα. Εκτός από αυτό, όταν θα σας εφαρμόζονται στον αλγόριθμο σε άλλον τομέα, το αποτέλεσμα συνήθως δεν είναι καλή. Σε σύγκριση με το ελέγχεται εκμάθησης, με βάση το λεξικό λειτουργεί χωρίς επιτήρηση εκμάθησης χρησιμοποιεί ένα λεξικό άποψη, το οποίο δεν απαιτεί την αποθήκευση ενός μεγάλου όγκου δεδομένων σώματος και εκπαίδευση - που κάνει ολόκληρης της διαδικασίας πολύ πιο γρήγορα. 

Το λεξικό Subjectivity MPQA (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), το οποίο είναι ένα από τα λεξικά που χρησιμοποιείται πιο συχνά subjectivity δημιουργείται μας [υπηρεσίας](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) . Υπάρχουν 5097 αρνητικές και 2533 θετικό λέξεις στο MPQA. Και όλες αυτές τις λέξεις με σχόλια ως πολικότητας υψηλές ή χαμηλές. Το ολόκληρη σώματος βρίσκεται στην περιοχή γενικά GNU δημόσια άδεια χρήσης. Η υπηρεσία web μπορούν να εφαρμοστούν σε οποιαδήποτε σύντομο προτάσεων, όπως tweets και Facebook δημοσιεύσεις. 

>Αυτή η υπηρεσία web μπορεί να χρησιμοποιηθεί από χρήστες – ενδεχομένως μέσω της εφαρμογής για κινητές συσκευές, μέσω μιας τοποθεσίας Web ή ακόμα και σε έναν τοπικό υπολογιστή για παράδειγμα. Αλλά ο σκοπός της υπηρεσίας web είναι επίσης για να λειτουργήσει ως παράδειγμα πώς Azure μηχανικής εκμάθησης μπορούν να χρησιμοποιηθούν για τη δημιουργία υπηρεσιών web επάνω σε κώδικα R. Με λίγα γραμμές κώδικα R και το κλικ ενός κουμπιού εντός Azure μηχανικής εκμάθησης Studio, μια έρευνα δυνατό να δημιουργηθούν με κωδικό R και δημοσιεύεται ως μια υπηρεσία web. Η υπηρεσία web μπορεί να, στη συνέχεια, δημοσιεύονται με το Azure Marketplace και που καταναλώνεται από τους χρήστες και τις συσκευές σε ολόκληρο τον κόσμο χωρίς ρύθμιση υποδομής από το συντάκτη της υπηρεσίας web.

##<a name="consumption-of-web-service"></a>Κατανάλωση της υπηρεσίας web

Τα δεδομένα εισόδου μπορεί να είναι οποιουδήποτε κειμένου, αλλά η υπηρεσία web λειτουργεί καλύτερα με σύντομο προτάσεων. Το αποτέλεσμα είναι μια αριθμητική τιμή μεταξύ 1 και -1. Κάθε τιμή κάτω από το 0 υποδηλώνει ότι είναι αρνητικός αριθμός, η άποψη του κειμένου θετικό εάν πάνω από το 0. Η απόλυτη τιμή του αποτελέσματος υποδηλώνει την ισχύ του το συσχετισμένο άποψη. 

>Αυτή η υπηρεσία, όπως φιλοξενούνται σε το Azure Marketplace, είναι μια υπηρεσία OData. αυτές μπορεί να ονομάζεται μέσω ΔΗΜΟΣΊΕΥΣΗ ή ΛΉΨΗ μεθόδους. 

Υπάρχουν πολλοί τρόποι του κατανάλωση η υπηρεσία με ένα αυτοματοποιημένο τρόπο (είναι μια εφαρμογή παράδειγμα [εδώ](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>Έναρξη κώδικα C# για κατανάλωση υπηρεσίας web:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



Τα δεδομένα εισόδου είναι "Σήμερα είναι μια καλή ημέρα". Το αποτέλεσμα είναι "1", το οποίο υποδεικνύει ένα θετικό άποψη που σχετίζεται με την πρόταση εισαγωγής. 

##<a name="creation-of-web-service"></a>Δημιουργία της υπηρεσίας web
>Αυτή η υπηρεσία web που δημιουργήθηκε με χρήση Azure μηχανικής εκμάθησης. Για μια δωρεάν δοκιμαστική έκδοση, καθώς και εισαγωγικά βίντεο σχετικά με τη δημιουργία δοκιμών και [δημοσίευσης υπηρεσίες web](machine-learning-publish-a-machine-learning-web-service.md), ανατρέξτε στο θέμα [azure.com/ml](http://azure.com/ml). Ακολουθεί ένα στιγμιότυπο οθόνης από την έρευνα που δημιουργήσατε τον κώδικα υπηρεσίας και παράδειγμα web για κάθε μία από τις λειτουργικές μονάδες μέσα την έρευνα.


Από μέσα σε Azure μηχανικής εκμάθησης, δημιουργήθηκε ένα νέο κενό έρευνας. Η παρακάτω εικόνα δείχνει τη ροή έρευνας ανάλυσης που βασίζεται στο λεξικό άποψη. Το αρχείο "sent_dict.csv" είναι το λεξικό subjectivity MPQA και έχει οριστεί ως ένα από τα δεδομένα εισόδου [Εκτέλεση]δέσμης ενεργειών R[execute-r-script]. Μια άλλη εισόδου είναι μια δείγματος αναθεώρηση από το σύνολο δεδομένων αναθεώρηση Amazon για δοκιμή, όπου θα σας έχει πραγματοποιηθεί επιλογής, τροποποίησης όνομα στήλης, και διαίρεση λειτουργίες. Χρησιμοποιούμε ένα πακέτο κατακερματισμός να αποθηκεύετε το λεξικό subjectivity στη μνήμη και να επιταχύνετε τη διαδικασία βαθμολογία κατά τον υπολογισμό. Ολόκληρο το κείμενο θα με διακριτικά από το πακέτο "tm" και σε σύγκριση με τη λέξη στο λεξικό άποψη. Τέλος, μια βαθμολογία θα υπολογιστεί προσθέτοντας το πάχος κάθε υποκειμενική λέξης στο κείμενο. 

###<a name="experiment-flow"></a>Ροή έρευνας:

![Πειραματιστείτε ροής][2]


####<a name="module-1"></a>Λειτουργική μονάδα 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Εγκατάσταση του πακέτου κατακερματισμός
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Περιορισμοί

Από μια προοπτική αλγόριθμο, βάσει λεξικό άποψη ανάλυσης είναι μια γενική άποψη εργαλείο ανάλυσης, το οποίο ενδέχεται να μην είναι καλύτερη από τη μέθοδο ταξινόμησης για συγκεκριμένα πεδία. Το πρόβλημα αρνητικοί αριθμοί δεν καλά εξετάζεται. Θα σας hardcode αρνητικοί αριθμοί πολλές λέξεις στο πρόγραμμα μας, αλλά καλύτερος τρόπος είναι χρησιμοποιώντας ένα λεξικό αρνητικοί αριθμοί και δημιουργήστε ορισμένοι κανόνες. Η υπηρεσία web εκτελεί καλύτερα σε σύντομο και απλό προτάσεων, όπως tweets και Facebook δημοσιεύσεις, από στη μεγάλη και σύνθετες προτάσεις όπως Amazon αναθεωρήσεις. 

##<a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
Για συνήθεις ερωτήσεις για την κατανάλωση της υπηρεσίας web ή δημοσίευσης με το Azure Marketplace, ανατρέξτε στο θέμα [εδώ](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 
