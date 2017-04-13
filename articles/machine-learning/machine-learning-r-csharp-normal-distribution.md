<properties 
    pageTitle="Κανονική κατανομή οικογένεια υπηρεσιών Web | Microsoft Azure" 
    description="Οικογένεια υπηρεσιών Web κανονική κατανομή" 
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

#<a name="normal-distribution-suite"></a>Κανονική κατανομή οικογένεια προγραμμάτων



Την οικογένεια κανονική κατανομή είναι ένα σύνολο των υπηρεσιών web δείγμα ([γεννήτρια]( https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Αριθμομηχανής]( https://datamarket.azure.com/dataset/aml_labs/ndq5), [Πιθανότητα Αριθμομηχανή]( https://datamarket.azure.com/dataset/aml_labs/ndp5)) που σας βοηθούν στη δημιουργία και διαχείριση κανονική κατανομή. Οι υπηρεσίες επιτρέπουν δημιουργεί μια ακολουθία κανονική κατανομή οποιοδήποτε μήκος, τον υπολογισμό quantiles από μια δεδομένη πιθανότητα και τον υπολογισμό πιθανότητα από μια δεδομένη quantile. Κάθε μία από τις υπηρεσίες εκπέμπει διαφορετικών αποτελεσμάτων με βάση την επιλεγμένη υπηρεσία (ανατρέξτε στο θέμα περιγραφή παρακάτω). Την οικογένεια κανονική κατανομή είναι με βάση το R συναρτήσεις qnorm rnorm και pnorm, που περιλαμβάνονται στο πακέτο στατιστικές R.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Αυτή η υπηρεσία web μπορεί να χρησιμοποιηθεί από χρήστες – ενδεχομένως μέσω της εφαρμογής για κινητές συσκευές, μέσω μιας τοποθεσίας Web ή ακόμα και σε έναν τοπικό υπολογιστή, για παράδειγμα. Αλλά ο σκοπός της υπηρεσίας web είναι επίσης για να λειτουργήσει ως παράδειγμα πώς Azure μηχανικής εκμάθησης μπορούν να χρησιμοποιηθούν για τη δημιουργία υπηρεσιών web επάνω σε κώδικα R. Με λίγα γραμμές κώδικα R και το κλικ ενός κουμπιού εντός Azure μηχανικής εκμάθησης Studio, μια έρευνα δυνατό να δημιουργηθούν με κωδικό R και δημοσιεύεται ως μια υπηρεσία web. Η υπηρεσία web μπορεί να, στη συνέχεια, δημοσιεύονται με το Azure Marketplace και που καταναλώνεται από τους χρήστες και τις συσκευές σε ολόκληρο τον κόσμο χωρίς ρύθμιση υποδομής από το συντάκτη της υπηρεσίας web.  
 

##<a name="consumption-of-web-service"></a>Κατανάλωση της υπηρεσίας web
Την οικογένεια κανονική κατανομή περιλαμβάνει τις ακόλουθες υπηρεσίες 3.

###<a name="normal-distribution-quantile-calculator"></a>Κανονική κατανομή Quantile Αριθμομηχανή
Αυτή η υπηρεσία αποδέχεται 4 ορίσματα της μια κανονική κατανομή και υπολογίζει το σχετικό quantile.

Τα ορίσματα εισόδου είναι οι εξής:

* p - μία πιθανότητα εκδήλωσης με κανονική κατανομή. 
* Ο όρος - τον μέσο κανονική κατανομή.
* SD - η τυπική απόκλιση κανονική κατανομή. 
* Πλευρά - L για κάτω πλευρά της κατανομής και U για το επάνω πλευράς της κατανομής.

Το αποτέλεσμα της υπηρεσίας είναι το quantile υπολογισμού που σχετίζεται με την πιθανότητα που δίνεται.

###<a name="normal-distribution-probability-calculator"></a>Κανονική κατανομή πιθανότητας Αριθμομηχανή
Αυτή η υπηρεσία αποδέχεται 4 ορίσματα της μια κανονική κατανομή και υπολογίζει την πιθανότητα που σχετίζεται.

Τα ορίσματα εισόδου είναι οι εξής:

* ερωτήσεις - ένα μεμονωμένο quantile του συμβάντος με κανονική κατανομή. 
* Ο όρος - τον μέσο κανονική κατανομή.
* SD - η τυπική απόκλιση κανονική κατανομή. 
* Πλευρά - L για κάτω πλευρά της κατανομής και U για το επάνω πλευράς της κατανομής.

Το αποτέλεσμα της υπηρεσίας είναι η υπολογισμού πιθανότητα που σχετίζεται με τη δεδομένη quantile.

###<a name="normal-distribution-generator"></a>Γεννήτρια κανονική κατανομή
Αυτή η υπηρεσία αποδέχεται 3 ορίσματα της μια κανονική κατανομή και δημιουργεί μια ακολουθία τυχαίων αριθμών που διανέμονται κανονικά. Τα ακόλουθα ορίσματα πρέπει να παρέχεται το μέσα στην πρόσκληση:

* n - ο αριθμός παρατηρήσεων. 
* ο όρος - τον μέσο κανονική κατανομή.
* SD - η τυπική απόκλιση κανονική κατανομή. 

Το αποτέλεσμα της υπηρεσίας είναι μια ακολουθία μήκος n, με βάση τα ορίσματα μέσο και sd κανονική κατανομή.

>Αυτή η υπηρεσία, όπως φιλοξενούνται σε το Azure Marketplace, είναι μια υπηρεσία OData. αυτές μπορεί να ονομάζεται μέσω ΔΗΜΟΣΊΕΥΣΗ ή ΛΉΨΗ μεθόδους. 

Υπάρχουν πολλοί τρόποι του κατανάλωση η υπηρεσία με ένα αυτοματοποιημένο τρόπο (παράδειγμα εφαρμογές είναι εδώ: [δημιουργίας](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Αριθμομηχανής πιθανότητα](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Αριθμομηχανής Quantile](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>Έναρξη κώδικα C# για κατανάλωση υπηρεσίας web:

###<a name="normal-distribution-quantile-calculator"></a>Κανονική κατανομή Quantile Αριθμομηχανή
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="normal-distribution-probability-calculator"></a>Κανονική κατανομή πιθανότητας Αριθμομηχανή
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="normal-distribution-generator"></a>Γεννήτρια κανονική κατανομή
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
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
>Αυτή η υπηρεσία web που δημιουργήθηκε με χρήση Azure μηχανικής εκμάθησης. Για μια δωρεάν δοκιμαστική έκδοση, καθώς και εισαγωγικά βίντεο σχετικά με τη δημιουργία δοκιμών και [δημοσίευσης υπηρεσίες web](machine-learning-publish-a-machine-learning-web-service.md), ανατρέξτε στο θέμα [azure.com/ml](http://azure.com/ml). 

Ακολουθεί ένα στιγμιότυπο οθόνης από την έρευνα που δημιουργήσατε τον κώδικα υπηρεσίας και παράδειγμα web για κάθε μία από τις λειτουργικές μονάδες μέσα την έρευνα.

###<a name="normal-distribution-quantile-calculator"></a>Κανονική κατανομή Quantile Αριθμομηχανή

Ροή έρευνας:

![Ροή έρευνας][2]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-probability-calculator"></a>Κανονική κατανομή πιθανότητας Αριθμομηχανή
Ροή έρευνας:

![Ροή έρευνας][3]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-generator"></a>Γεννήτρια κανονική κατανομή
Ροή έρευνας:

![Ροή έρευνας][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Περιορισμοί 

Αυτά είναι πολύ απλής παραδείγματα γύρω από την κανονική κατανομή. Όπως φαίνεται από το παραπάνω παράδειγμα κώδικα, μικρή αλίευση σφάλματος έχει υλοποιηθεί.

##<a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
Για συνήθεις ερωτήσεις για την κατανάλωση της υπηρεσίας web ή δημοσίευσης με το Azure Marketplace, ανατρέξτε στο θέμα [εδώ](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png
 
