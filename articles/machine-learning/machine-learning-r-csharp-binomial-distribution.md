<properties 
    pageTitle="Οικογένεια διωνυμική κατανομή | Microsoft Azure" 
    description="Οικογένεια διωνυμική κατανομή" 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 


#<a name="binomial-distribution-suite"></a>Οικογένεια διωνυμική κατανομή




Την οικογένεια διωνυμική κατανομή είναι ένα σύνολο των υπηρεσιών web δείγμα ([Διωνυμική δημιουργίας](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Αριθμομηχανής πιθανότητα]( https://datamarket.azure.com/dataset/aml_labs/bdp4), [Αριθμομηχανής Quantile]( https://datamarket.azure.com/dataset/aml_labs/bdq5)) που σας βοηθούν στη δημιουργία και χειρισμός διωνυμική κατανομή. Οι υπηρεσίες επιτρέπουν χρησιμοποιώντας μια διωνυμική κατανομή ακολουθία οποιοδήποτε μήκος, τον υπολογισμό quantiles εκτός δεδομένο πιθανότητα και τον υπολογισμό πιθανότητα από μια δεδομένη quantile. Κάθε μία από τις υπηρεσίες εκπέμπει διαφορετικών αποτελεσμάτων με βάση την επιλεγμένη υπηρεσία (ανατρέξτε στο θέμα περιγραφή παρακάτω). Την οικογένεια διωνυμική κατανομή είναι με βάση το R συναρτήσεις qbinom rbinom και pbinom, που περιλαμβάνονται στο πακέτο στατιστικές R. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Αυτές τις υπηρεσίες web θα μπορούσε να καταναλωθεί από τους χρήστες – ενδεχομένως απευθείας στην αγορά του, μέσω εφαρμογής για κινητές συσκευές, μέσω μιας τοποθεσίας Web ή ακόμα και σε έναν τοπικό υπολογιστή, για παράδειγμα. Αλλά ο σκοπός της υπηρεσίας web είναι επίσης για να λειτουργήσει ως παράδειγμα πώς Azure μηχανικής εκμάθησης μπορούν να χρησιμοποιηθούν για τη δημιουργία υπηρεσιών web επάνω σε κώδικα R. Με λίγα γραμμές κώδικα R και το κλικ ενός κουμπιού εντός Azure μηχανικής εκμάθησης Studio, μια έρευνα δυνατό να δημιουργηθούν με κωδικό R και δημοσιεύεται ως μια υπηρεσία web. Η υπηρεσία web, στη συνέχεια, μπορεί να δημοσιευτεί με το Azure Marketplace και να καταναλώνεται από τους χρήστες και τις συσκευές σε ολόκληρο τον κόσμο – χωρίς ρύθμιση υποδομής από το συντάκτη της υπηρεσίας web είναι απαραίτητο.

##<a name="consumption-of-web-service"></a>Κατανάλωση της υπηρεσίας web
Την οικογένεια διωνυμική κατανομή περιλαμβάνει τις ακόλουθες υπηρεσίες 3.

###<a name="binomial-distribution-quantile-calculator"></a>Αριθμομηχανής Quantile διωνυμική κατανομή
Αυτή η υπηρεσία αποδέχεται 4 ορίσματα της μια κανονική κατανομή και υπολογίζει το σχετικό quantile.
Τα ορίσματα εισόδου είναι οι εξής:

- p - μία συγκεντρωτική πιθανότητα πολλές δοκιμές.  
- μέγεθος - ο αριθμός των δοκιμών.
- η συνάρτηση PROB - η πιθανότητα επιτυχίας σε μια δοκιμαστική έκδοση.
- Πλευρά - L για κάτω πλευρά της κατανομής, U για το επάνω πλευράς της κατανομής. 

Το αποτέλεσμα της υπηρεσίας είναι το quantile υπολογισμού που σχετίζεται με την πιθανότητα που δίνεται.

###<a name="binomial-distribution-probability-calculator"></a>Διωνυμική κατανομή πιθανότητας Αριθμομηχανή
Αυτή η υπηρεσία αποδέχεται 4 ορίσματα της μια διωνυμική κατανομή και υπολογίζει την πιθανότητα που σχετίζεται.
Τα ορίσματα εισόδου είναι οι εξής:

- ερωτήσεις - ένα μεμονωμένο quantile του συμβάντος με διωνυμική κατανομή. 
- μέγεθος - ο αριθμός των δοκιμών.
- η συνάρτηση PROB - η πιθανότητα επιτυχίας σε μια δοκιμαστική έκδοση.
- πλευρά - L για κάτω πλευρά της κατανομής, U για επάνω πλευρά της διανομής ή E που είναι ίσο με έναν αριθμό επιτυχιών.

Το αποτέλεσμα της υπηρεσίας είναι η υπολογισμού πιθανότητα που σχετίζεται με τη δεδομένη quantile.

###<a name="binomial-distribution-generator"></a>Γεννήτρια διωνυμική κατανομή
Αυτή η υπηρεσία αποδέχεται 3 ορίσματα της μια διωνυμική κατανομή και δημιουργεί μια ακολουθία τυχαίων αριθμών που διανέμονται binomially. Τα ακόλουθα ορίσματα πρέπει να παρέχεται το μέσα στην πρόσκληση:

- n - αριθμός παρατηρήσεων. 
- μέγεθος - αριθμός δοκιμών.
- η συνάρτηση PROB - πιθανότητα επιτυχίας.

Το αποτέλεσμα της υπηρεσίας είναι μια ακολουθία μήκος n με μια διωνυμική κατανομή με βάση τα ορίσματα μέγεθος και η συνάρτηση prob.

>Αυτή η υπηρεσία, όπως φιλοξενούνται σε το Azure Marketplace, είναι μια υπηρεσία OData. αυτές μπορεί να ονομάζεται μέσω ΔΗΜΟΣΊΕΥΣΗ ή ΛΉΨΗ μεθόδους. 

Υπάρχουν πολλοί τρόποι του κατανάλωση η υπηρεσία με ένα αυτοματοποιημένο τρόπο (παράδειγμα εφαρμογές είναι εδώ: [δημιουργίας](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Αριθμομηχανής πιθανότητα](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Αριθμομηχανής Quantile](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Έναρξη κώδικα C# για κατανάλωση υπηρεσίας web:

###<a name="binomial-distribution-quantile-calculator"></a>Αριθμομηχανής Quantile διωνυμική κατανομή
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="binomial-distribution-probability-calculator"></a>Διωνυμική κατανομή πιθανότητας Αριθμομηχανή
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="binomial-distribution-generator"></a>Γεννήτρια διωνυμική κατανομή
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

###<a name="binomial-distribution-quantile-calculator"></a>Αριθμομηχανής Quantile διωνυμική κατανομή

![Δημιουργία χώρου εργασίας][4]

####<a name="module-1"></a>Λειτουργική μονάδα 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####<a name="module-2"></a>Λειτουργική μονάδα 2:

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###<a name="binomial-distribution-probability-calculator"></a>Διωνυμική κατανομή πιθανότητας Αριθμομηχανή

![Δημιουργία χώρου εργασίας][5]

####<a name="module-1"></a>Λειτουργική μονάδα 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####<a name="module-2"></a>Λειτουργική μονάδα 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###<a name="binomial-distribution-generator"></a>Γεννήτρια διωνυμική κατανομή

![Δημιουργία χώρου εργασίας][6]

####<a name="module-1"></a>Λειτουργική μονάδα 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####<a name="module-2"></a>Λειτουργική μονάδα 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Περιορισμοί 
Αυτά είναι πολύ απλής παραδείγματα περιβάλλοντος η διωνυμική κατανομή. Όπως φαίνεται από το παραπάνω παράδειγμα κώδικα, μικρή αλίευση σφάλματος έχει υλοποιηθεί.

##<a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
Για συνήθεις ερωτήσεις για την κατανάλωση της υπηρεσίας web ή δημοσίευσης με το Azure Marketplace, ανατρέξτε στο θέμα [εδώ](machine-learning-marketplace-faq.md).


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 
