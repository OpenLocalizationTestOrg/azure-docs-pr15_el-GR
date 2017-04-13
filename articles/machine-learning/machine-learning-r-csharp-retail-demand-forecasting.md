<properties 
    pageTitle="Προβλέψεις - ETS + STL | Microsoft Azure" 
    description="Προβλέψεις - ETS + STL" 
    services="machine-learning" 
    documentationCenter="" 
    authors="xueshanz" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="yijichen"/> 

#<a name="forecasting---ets--stl"></a>Προβλέψεις - ETS + STL  

Αυτή η [υπηρεσία web]( https://datamarket.azure.com/dataset/aml_labs/demand_forecast) υλοποιεί μοντέλα εποχιακά αποδόμησης τάση (STL) και Εκθετικής εξομάλυνσης (ETS) για την παραγωγή προβλέψεων με βάση τα δεδομένα ιστορικού που παρέχεται από το χρήστη. Η ζήτηση για ένα συγκεκριμένο προϊόν, θα αυξήσετε αυτό το έτος; Μπορεί να να προβλέψει μου Πωλήσεις προϊόντων για την περίοδο των Χριστουγέννων, έτσι, ώστε να μπορούν να αποτελεσματικό σχεδιασμό απόθεμα μου; Τα μοντέλα πρόβλεψης είναι κατάλληλη για να απαντούν σε ερωτήσεις όπως. Αυτά τα μοντέλα δεδομένο τα τελευταία δεδομένα, εξετάστε κρυφών τάσεις και εποχικότητα πρόβλεψη μελλοντικών τάσεις. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
 
>Αυτή η υπηρεσία web μπορεί να χρησιμοποιηθεί από χρήστες – ενδεχομένως μέσω της εφαρμογής για κινητές συσκευές, μέσω μιας τοποθεσίας Web ή ακόμα και σε έναν τοπικό υπολογιστή, για παράδειγμα. Αλλά ο σκοπός της υπηρεσίας web είναι επίσης για να λειτουργήσει ως παράδειγμα πώς Azure μηχανικής εκμάθησης μπορούν να χρησιμοποιηθούν για τη δημιουργία υπηρεσιών web επάνω σε κώδικα R. Με λίγα γραμμές κώδικα R και το κλικ ενός κουμπιού εντός Azure μηχανικής εκμάθησης Studio, μια έρευνα δυνατό να δημιουργηθούν με κωδικό R και δημοσιεύεται ως μια υπηρεσία web. Η υπηρεσία web μπορεί να, στη συνέχεια, δημοσιεύονται με το Azure Marketplace και που καταναλώνεται από τους χρήστες και τις συσκευές σε ολόκληρο τον κόσμο χωρίς ρύθμιση υποδομής από το συντάκτη της υπηρεσίας web.  
 
##<a name="consumption-of-web-service"></a>Κατανάλωση της υπηρεσίας web 

Αυτή η υπηρεσία αποδέχεται 4 ορίσματα και υπολογίζει τις προβλέψεις.
Τα ορίσματα εισόδου είναι οι εξής:

* Συχνότητα - υποδεικνύει τη συχνότητα τα ανεπεξέργαστα δεδομένα (Ημερήσια/Εβδομαδιαία/Μηνιαία/τριμηνιαίες/ετήσια).
* Ορίζοντας - μέλλον πρόβλεψης χρονικό πλαίσιο.
* Ημερομηνία - Προσθήκη στα νέα δεδομένα σειράς χρόνου για το χρόνο.
* Τιμή - Προσθήκη στο τις νέες τιμές ώρας σειράς δεδομένων.

Το αποτέλεσμα της υπηρεσίας είναι οι υπολογισμένες τιμές πρόβλεψης.
 
Δείγμα εισαγωγής μπορεί να είναι: 

* Συχνότητα - 12
* Ορίζοντας - 12
* Ημερομηνία - 15/1/2012, 15/2/2012, 15/3/2012, 15/4/2012, 15/5/2012, 15/6/2012, 15/7/2012, 8 / 15/2012, 15/9/2012, 15/10/2012, 15/11/2012, 12/15/2012 15/1/2013, 15/2/2013, 15/3/2013; 15/4/2013; 15/5/2013; 15/6/2013; 15/7/2013, 8 / 15/2013, 15/9/2013, 15/10/2013; 15/11/2013, 15/12/2013; 15/1/2014, 15/2/2014, 15/3/2014; 15/4/2014; 15/5/2014; 15/6/2014; 15/7/2014, 8 / 15/2014; 15/9/2014
* Τιμή - 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509

>Αυτή η υπηρεσία, όπως φιλοξενούνται σε το Azure Marketplace, είναι μια υπηρεσία OData. αυτές μπορεί να ονομάζεται μέσω ΔΗΜΟΣΊΕΥΣΗ ή ΛΉΨΗ μεθόδους. 

Υπάρχουν πολλοί τρόποι του κατανάλωση η υπηρεσία με ένα αυτοματοποιημένο τρόπο (είναι μια εφαρμογή παράδειγμα [εδώ](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Έναρξη κώδικα C# για κατανάλωση υπηρεσίας web:

    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };         var json = JsonConvert.SerializeObject(input);
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

Από μέσα σε Azure μηχανικής εκμάθησης, δημιουργήθηκε ένα νέο κενό έρευνας. Δείγμα δεδομένων εισόδου φορτώθηκε με ένα σχήμα προκαθορισμένης δεδομένων. Συνδεδεμένο με το σχήμα δεδομένων είναι μια [Δέσμη ενεργειών εκτέλεση R] [ execute-r-script] λειτουργική μονάδα, που δημιουργεί STL και ETS προβλέψεις μοντέλα χρησιμοποιώντας 'stl', 'ets' και 'πρόβλεψης' λειτουργεί από R. 

###<a name="experiment-flow"></a>Ροή έρευνας:

![Ροή έρευνας][2]

####<a name="module-1"></a>Λειτουργική μονάδα 1:
 
    # Add in the CSV file with the data in the format shown below 
![Δείγμα δεδομένων][3]   

####<a name="module-2"></a>Λειτουργική μονάδα 2:

    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # Fit a time series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- stl(train_ts,  s.window="periodic")
    train_model <- forecast(fit1, h = data$horizon, method = 'ets')
    plot(train_model)
    
    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # Data output
    maml.mapOutputPort("data.forecast");

##<a name="limitations"></a>Περιορισμοί 

Αυτό είναι ένα πολύ απλό παράδειγμα για ETS + STL προβλέψεις. Όπως φαίνεται από το παραπάνω παράδειγμα κώδικα, χωρίς αλίευση σφαλμάτων εφαρμόζεται και την υπηρεσία προϋποθέτει ότι όλες τις μεταβλητές είναι συνεχής/θετικές τιμές και τη συχνότητα πρέπει να είναι ένας ακέραιος αριθμός που είναι μεγαλύτερο από το 1. Το μήκος της την ημερομηνία και την τιμή διανύσματα πρέπει να είναι η ίδια και το μήκος της την χρονολογική σειρά πρέπει να είναι μεγαλύτερο από 2 * συχνότητα. Η μεταβλητή ημερομηνία πρέπει να συμφωνεί με τη μορφή ' ηη/μμ/εεεε'.

##<a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
Για συνήθεις ερωτήσεις για την κατανάλωση της υπηρεσίας web ή δημοσίευσης με το Azure Marketplace, ανατρέξτε στο θέμα [εδώ](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img1.png
[2]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img2.png
[3]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
