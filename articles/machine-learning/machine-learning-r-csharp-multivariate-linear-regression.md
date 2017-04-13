<properties 
    pageTitle="Multivariate γραμμική παλινδρόμηση | Microsoft Azure" 
    description="Multivariate γραμμικής παλινδρόμησης" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Multivariate γραμμικής παλινδρόμησης   
 

 
Ας υποθέσουμε ότι έχετε ένα σύνολο δεδομένων και θα θέλατε να πρόβλεψης γρήγορα μια μεταβλητή εξαρτώνται από (y) για κάθε άτομο (i) που βασίζονται σε ανεξάρτητων μεταβλητών. Γραμμικής παλινδρόμησης είναι μια τεχνική δημοφιλείς στατιστικές που χρησιμοποιείται για το εν λόγω προβλέψεων. Δείτε τη μεταβλητή εξαρτώνται από y λαμβάνεται μιας συνεχούς τιμής.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Ένα απλό σενάριο ενδέχεται να όπου το ερευνητές προσπαθεί να προβλέψει το πάχος ενός ατόμου (y) με βάση το ύψος (x). Μια πιο σύνθετη σενάριο μπορεί να είναι όπου το ερευνητές έχει πρόσθετες πληροφορίες για το άτομο (όπως πάχος, φύλο, αγωνιστικά) και προσπαθεί να προβλέψει το πάχος του ατόμου. Αυτή η [υπηρεσία web]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) χωράει το μοντέλο γραμμικής παλινδρόμησης για τα δεδομένα και εξόδους προβλεπόμενης τιμής τιμή (y) για κάθε μία από τις παρατηρήσεις στα δεδομένα.

>Αυτή η υπηρεσία web μπορεί να χρησιμοποιηθεί από χρήστες – ενδεχομένως μέσω της εφαρμογής για κινητές συσκευές, μέσω μιας τοποθεσίας Web ή ακόμα και σε έναν τοπικό υπολογιστή, για παράδειγμα. Αλλά ο σκοπός της υπηρεσίας web είναι επίσης για να λειτουργήσει ως παράδειγμα πώς Azure μηχανικής εκμάθησης μπορούν να χρησιμοποιηθούν για τη δημιουργία υπηρεσιών web επάνω σε κώδικα R. Με λίγα γραμμές κώδικα R και το κλικ ενός κουμπιού εντός Azure μηχανικής εκμάθησης Studio, μια έρευνα δυνατό να δημιουργηθούν με κωδικό R και δημοσιεύεται ως μια υπηρεσία web. Η υπηρεσία web μπορεί να, στη συνέχεια, δημοσιεύονται με το Azure Marketplace και που καταναλώνεται από τους χρήστες και τις συσκευές σε ολόκληρο τον κόσμο χωρίς ρύθμιση υποδομής από το συντάκτη της υπηρεσίας web.  

##<a name="consumption-of-web-service"></a>Κατανάλωση της υπηρεσίας web  
Αυτή η υπηρεσία web παρέχει τις προβλεπόμενες τιμές της μεταβλητής εξαρτώνται από τις ανεξάρτητες μεταβλητές για όλες τις παρατηρήσεις με βάση. Η υπηρεσία web αναμένει του τελικού χρήστη για εισαγωγή δεδομένων ως συμβολοσειρά όπου οι γραμμές διαχωρίζονται με κόμματα (,) και οι στήλες χωρίζονται με ελληνικά ερωτηματικά (;). Η υπηρεσία web αναμένει 1 γραμμή τη φορά και αναμένει να είναι η μεταβλητή εξαρτώνται από την πρώτη στήλη. Ένα σύνολο δεδομένων παράδειγμα μπορεί να μοιάζει κάπως έτσι:

![Δείγμα δεδομένων][1]

Παρατηρήσεις χωρίς μια μεταβλητή εξαρτώνται από πρέπει να εισάγονται ως "ΔΥ" του y. Τα δεδομένα εισόδου για το παραπάνω σύνολο δεδομένων θα είναι το ακόλουθο μήνυμα: "10; 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1, NA; 3; 4". Το αποτέλεσμα είναι η προβλεπόμενη τιμή για κάθε μία από τις γραμμές με βάση τις ανεξάρτητες μεταβλητές. 

>Αυτή η υπηρεσία, όπως φιλοξενούνται σε το Azure Marketplace, είναι μια υπηρεσία OData. αυτές μπορεί να ονομάζεται μέσω ΔΗΜΟΣΊΕΥΣΗ ή ΛΉΨΗ μεθόδους. 

Υπάρχουν πολλοί τρόποι του κατανάλωση η υπηρεσία με ένα αυτοματοποιημένο τρόπο (είναι μια εφαρμογή παράδειγμα [εδώ](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Έναρξη κώδικα C# για κατανάλωση υπηρεσίας web:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




##<a name="creation-of-web-service"></a>Δημιουργία της υπηρεσίας web  
>Αυτή η υπηρεσία web που δημιουργήθηκε με χρήση Azure μηχανικής εκμάθησης. Για μια δωρεάν δοκιμαστική έκδοση, καθώς και εισαγωγικά βίντεο σχετικά με τη δημιουργία δοκιμών και [δημοσίευσης υπηρεσίες web](machine-learning-publish-a-machine-learning-web-service.md), ανατρέξτε στο θέμα [azure.com/ml](http://azure.com/ml). Ακολουθεί ένα στιγμιότυπο οθόνης από την έρευνα που δημιουργήσατε τον κώδικα υπηρεσίας και παράδειγμα web για κάθε μία από τις λειτουργικές μονάδες μέσα την έρευνα.


Από μέσα σε Azure μηχανικής εκμάθησης, μια νέα κενή έρευνας δημιουργήθηκε και δύο [Εκτέλεση δέσμης ενεργειών R] [ execute-r-script] λειτουργικές μονάδες έχουν τα μηνύματα μεταφέρονται σε χώρο εργασίας. Αυτή η υπηρεσία web εκτελείται μια έρευνα Azure μηχανικής εκμάθησης με ένα υποκείμενο δέσμη ενεργειών R. Υπάρχουν δύο μέρη σε αυτήν την έρευνα: Ορισμός σχήματος, και εκπαίδευση μοντέλο + βαθμολογίας. Την πρώτη λειτουργική μονάδα ορίζει τη δομή αναμενόμενο το εισαγωγής στο σύνολο δεδομένων, όπου η πρώτη μεταβλητή είναι η μεταβλητή εξαρτώνται από και τις υπόλοιπες μεταβλητές είναι ανεξάρτητα. Η δεύτερη λειτουργική μονάδα σχετίζεται με ένα μοντέλο γενικής χρήσης γραμμικής παλινδρόμησης για τα δεδομένα εισόδου.  
  
![Ροή έρευνας][3]

####<a name="module-1"></a>Λειτουργική μονάδα 1:
 
####<a name="schema-definition"></a>Ορισμός σχήματος  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>Λειτουργική μονάδα 2:
####<a name="lm-modeling"></a>Μοντελοποίηση LM   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Περιορισμοί
Αυτό είναι ένα πολύ απλό παράδειγμα πολλών υπηρεσίας web γραμμικής παλινδρόμησης. Όπως φαίνεται από το παραπάνω παράδειγμα κώδικα, χωρίς αλίευση σφαλμάτων εφαρμόζεται και την υπηρεσία προϋποθέτει ότι όλα τα στοιχεία είναι μια συνεχόμενη μεταβλητή (δεν έλαβε δυνατότητες επιτρέπονται), ως την υπηρεσία μόνο εισόδων αριθμητικές τιμές την ώρα από τη δημιουργία της αυτήν την υπηρεσία web. Επίσης, η υπηρεσία αυτήν τη στιγμή χειρίζεται μέγεθος περιορισμένη δεδομένων, προθεσμία της φύσης αίτηση/απάντηση της κλήσης στην υπηρεσία web και το γεγονός ότι το μοντέλο είναι που χωρά κάθε φορά που ονομάζεται την υπηρεσία web. 

##<a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
Για συνήθεις ερωτήσεις για την κατανάλωση της υπηρεσίας web ή δημοσίευσης με το Azure Marketplace, ανατρέξτε στο θέμα [εδώ](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
