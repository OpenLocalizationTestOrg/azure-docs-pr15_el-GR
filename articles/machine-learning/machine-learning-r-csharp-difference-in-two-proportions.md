<properties 
    pageTitle="Διαφορά σε δοκιμή αναλογίες | Microsoft Azure" 
    description="Διαφορά στο αναλογίες δοκιμής" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Διαφορά στο αναλογίες δοκιμής


Διαφέρουν δύο αναλογίες στατιστικά; Ας υποθέσουμε ότι ένας χρήστης θέλει να συγκρίνετε δύο ταινίες για να καθορίσετε εάν ένα ταινίας περιλαμβάνει σημαντικά υψηλότερη ποσοστό 'αρέσει' όταν σε σύγκριση με το άλλο. Με ένα μεγάλο δείγμα, ενδέχεται να υπάρχουν στατιστικά σημαντική διαφορά μεταξύ των αναλογιών 0,50 και 0,51. Με ένα μικρό δείγμα, ίσως να μην υπάρχουν αρκετά δεδομένα για να προσδιορίσετε εάν αυτές τις αναλογίες είναι στην πραγματικότητα διαφορετικά. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Αυτή η [υπηρεσία web]( https://datamarket.azure.com/dataset/aml_labs/prop_test) πραγματοποιεί μια έλεγχος υποθέσεων της διαφοράς σε δύο αναλογίες που βασίζονται σε εισαγωγή από το χρήστη από τον αριθμό των επιτυχιών και ο συνολικός αριθμός των δοκιμές για τις ομάδες 2 σύγκρισης. Σε ένα σενάριο πιθανές, μέσα σε μια εφαρμογή σύγκρισης ταινίας, που ενημερώνει το χρήστη εάν ένα από τα ταινίες είναι πραγματικά 'δηλώσει ότι σας αρέσει' πιο συχνά από την άλλη, με βάση τους χαρακτηρισμούς ταινίας μπορεί να ονομάζεται από αυτήν την υπηρεσία web.

>Αυτή η υπηρεσία web μπορεί να χρησιμοποιηθεί από χρήστες – ενδεχομένως μέσω της εφαρμογής για κινητές συσκευές, μέσω μιας τοποθεσίας Web ή ακόμα και σε έναν τοπικό υπολογιστή, για παράδειγμα. Αλλά ο σκοπός της υπηρεσίας web είναι επίσης για να λειτουργήσει ως παράδειγμα πώς Azure μηχανικής εκμάθησης μπορούν να χρησιμοποιηθούν για τη δημιουργία υπηρεσιών web επάνω σε κώδικα R. Με λίγα γραμμές κώδικα R και το κλικ ενός κουμπιού εντός Azure μηχανικής εκμάθησης Studio, μια έρευνα δυνατό να δημιουργηθούν με κωδικό R και να δημοσιευτεί ως υπηρεσία web. Η υπηρεσία web μπορεί να, στη συνέχεια, δημοσιεύονται με το Azure Marketplace και που καταναλώνεται από τους χρήστες και τις συσκευές σε ολόκληρο τον κόσμο χωρίς ρύθμιση υποδομής από το συντάκτη της υπηρεσίας web.


##<a name="consumption-of-web-service"></a>Κατανάλωση της υπηρεσίας web

Αυτή η υπηρεσία αποδέχεται 4 ορίσματα και μια υπόθεση δοκιμή των αναλογιών.

Τα ορίσματα εισόδου είναι οι εξής:

* Successes1 - αριθμός γεγονότων επιτυχιών στο δείγμα 1.
* Successes2 - αριθμός γεγονότων επιτυχιών στο δείγμα 2.
* Total1 - μέγεθος δείγματος 1.
* Total2 - μέγεθος δείγματος 2.

Το αποτέλεσμα της υπηρεσίας είναι το αποτέλεσμα της υπόθεσης δοκιμή μαζί με την τιμή p στατιστικής, df, chi-square και αναλογία στο δείγμα 1/2 και διάστημα εμπιστοσύνης ορίων.

>Αυτή η υπηρεσία, όπως φιλοξενούνται σε το Azure Marketplace, είναι μια υπηρεσία OData. αυτές μπορεί να ονομάζεται μέσω ΔΗΜΟΣΊΕΥΣΗ ή ΛΉΨΗ μεθόδους. 

Υπάρχουν πολλοί τρόποι του κατανάλωση η υπηρεσία με ένα αυτοματοποιημένο τρόπο (είναι μια εφαρμογή παράδειγμα [εδώ](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Έναρξη κώδικα C# για κατανάλωση υπηρεσίας web:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
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

Από μέσα σε Azure μηχανικής εκμάθησης, μια νέα κενή έρευνας που δημιουργήθηκε με δύο [Εκτέλεση δέσμης ενεργειών R] [ execute-r-script] λειτουργικές μονάδες. Στην πρώτη λειτουργική μονάδα που ορίζονται από το σχήμα των δεδομένων, κατά το δεύτερο λειτουργική μονάδα χρησιμοποιεί την εντολή prop.test μέσα R για να εκτελέσετε τον έλεγχο υποθέσεων για 2 αναλογίες. 


###<a name="experiment-flow"></a>Ροή έρευνας:

![Ροή έρευνας][2]


####<a name="module-1"></a>Λειτουργική μονάδα 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>Λειτουργική μονάδα 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Περιορισμοί 

Αυτό είναι ένα πολύ απλό παράδειγμα για μια δοκιμαστική της διαφοράς σε 2 αναλογίες. Όπως φαίνεται από το παραπάνω παράδειγμα κώδικα, χωρίς αλίευση σφαλμάτων εφαρμόζεται και την υπηρεσία προϋποθέτει ότι είναι συνεχής όλες τις μεταβλητές.

##<a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
Για συνήθεις ερωτήσεις για την κατανάλωση της υπηρεσίας web ή δημοσίευσης με το Azure Marketplace, ανατρέξτε στο θέμα [εδώ](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
