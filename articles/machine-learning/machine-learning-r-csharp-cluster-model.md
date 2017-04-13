<properties 
    pageTitle="Σύμπλεγμα μοντέλο | Microsoft Azure" 
    description="Μοντέλο συμπλέγματος" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Μοντέλο συμπλέγματος    

Πώς θα σας μπορεί να προβλέψει ομάδες των πιστωτικών cardholders συμπεριφορών προκειμένου να μειωθεί ο κίνδυνος χρέωση απενεργοποίηση οι εκδότες πιστωτικών καρτών; Τρόπος να ορισμού ομάδες των χαρακτηριστικών προσωπικότητα των υπαλλήλων προκειμένου να βελτιώσουν τις επιδόσεις στην εργασία; Πώς να ιατρών ταξινομήσετε ασθενών σε ομάδες με βάση τα χαρακτηριστικά τους ασθενειών; Κατ ' αρχήν, όλες αυτές τις ερωτήσεις μπορεί να είναι να απαντηθεί μέσω σύμπλεγμα ανάλυσης.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Ανάλυση σύμπλεγμα ταξινομεί ένα σύνολο παρατηρήσεων σε δύο ή περισσότερες αμοιβαία αποκλειόμενα άγνωστη ομάδες με βάση τους συνδυασμούς μεταβλητών. Ο σκοπός της ανάλυσης σύμπλεγμα είναι εντοπισμός ενός συστήματος οργάνωσης παρατηρήσεις, συνήθως άτομα ή τα χαρακτηριστικά τους, σε ομάδες, όπου τα μέλη των ομάδων κοινή χρήση ιδιότητες κοινά. Αυτή η [υπηρεσία](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) χρησιμοποιεί τη μεθοδολογία σημαίνει K, που χρησιμοποιείται συχνά συμπλέγματος τεχνική, σύμπλεγμα αυθαίρετο δεδομένα σε ομάδες. Αυτή η υπηρεσία web λαμβάνει τα δεδομένα και ο αριθμός των k συμπλεγμάτων ως είσοδο και δημιουργεί προβλέψεων των οποίων των ομάδων k στην οποία ανήκει κάθε παρατηρήσεις. 

>Αυτή η υπηρεσία web μπορεί να χρησιμοποιηθεί από χρήστες – ενδεχομένως μέσω της εφαρμογής για κινητές συσκευές, μέσω μιας τοποθεσίας Web ή ακόμα και σε έναν τοπικό υπολογιστή, για παράδειγμα. Αλλά ο σκοπός της υπηρεσίας web είναι επίσης για να λειτουργήσει ως παράδειγμα πώς Azure μηχανικής εκμάθησης μπορούν να χρησιμοποιηθούν για τη δημιουργία υπηρεσιών web επάνω σε κώδικα R. Με λίγα γραμμές κώδικα R και το κλικ ενός κουμπιού εντός Azure μηχανικής εκμάθησης Studio, μια έρευνα δυνατό να δημιουργηθούν με κωδικό R και δημοσιεύεται ως μια υπηρεσία web. Η υπηρεσία web μπορεί να, στη συνέχεια, δημοσιεύονται με το Azure Marketplace και που καταναλώνεται από τους χρήστες και τις συσκευές σε ολόκληρο τον κόσμο χωρίς ρύθμιση υποδομής από το συντάκτη της υπηρεσίας web.  

##<a name="consumption-of-web-service"></a>Κατανάλωση της υπηρεσίας web   
Αυτή η υπηρεσία web ομαδοποιεί τα δεδομένα σε ένα σύνολο ομάδων k και εξέρχεται ομάδας ανάθεσης για κάθε γραμμή. Η υπηρεσία web αναμένει του τελικού χρήστη για εισαγωγή δεδομένων ως συμβολοσειρά όπου οι γραμμές διαχωρίζονται με κόμματα (,) και οι στήλες χωρίζονται με ελληνικά ερωτηματικά (;). Η υπηρεσία web αναμένει 1 γραμμή τη φορά. Ένα σύνολο δεδομένων παράδειγμα μπορεί να μοιάζει κάπως έτσι:

![Δείγμα δεδομένων][1]

Ας υποθέσουμε ότι ο χρήστης θέλει να διαχωρίσετε αυτών των δεδομένων σε 3 αμοιβαία αποκλειόμενων ομάδων. Τα δεδομένα εισόδου για το παραπάνω σύνολο δεδομένων θα είναι τα εξής: τιμή = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". Το αποτέλεσμα είναι η συμμετοχή σε ομάδα προβλεπόμενες για κάθε μία από τις γραμμές.

>Αυτή η υπηρεσία, όπως φιλοξενούνται σε το Azure Marketplace, είναι μια υπηρεσία OData. αυτές μπορεί να ονομάζεται μέσω ΔΗΜΟΣΊΕΥΣΗ ή ΛΉΨΗ μεθόδους. 

Υπάρχουν πολλοί τρόποι του κατανάλωση η υπηρεσία με ένα αυτοματοποιημένο τρόπο (είναι μια εφαρμογή παράδειγμα [εδώ](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Έναρξη κώδικα C# για κατανάλωση υπηρεσίας web:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

Από μέσα σε Azure μηχανικής εκμάθησης, μια νέα κενή έρευνας δημιουργήθηκε και δύο [Εκτέλεση δέσμης ενεργειών R] [ execute-r-script] λειτουργικές μονάδες τα μηνύματα μεταφέρονται σε χώρο εργασίας. Το σχήμα των δεδομένων που δημιουργήθηκε με μια απλή [Δέσμη ενεργειών εκτέλεση R][execute-r-script]. Στη συνέχεια, το σχήμα των δεδομένων συνδέθηκε με την ενότητα μοντέλο σύμπλεγμα, δημιουργηθεί ξανά με μια [Δέσμη ενεργειών εκτέλεση R][execute-r-script]. Στη [Δέσμη ενεργειών εκτέλεση R] [ execute-r-script] χρησιμοποιείται για το σύμπλεγμα μοντέλο, την υπηρεσία web, στη συνέχεια, χρησιμοποιεί τη συνάρτηση "k-σημαίνει", η οποία είναι προ-δημιουργημένα σε την [Εκτέλεση δέσμης ενεργειών R] [ execute-r-script] του Azure μηχανικής εκμάθησης.    
   

     
![Ροή έρευνας][3]

####<a name="module-1"></a>Λειτουργική μονάδα 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>Λειτουργική μονάδα 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Περιορισμοί
Αυτό είναι ένα πολύ απλό παράδειγμα μιας υπηρεσίας web συμπλέγματος. Όπως φαίνεται από το παραπάνω παράδειγμα κώδικα, χωρίς αλίευση σφαλμάτων εφαρμόζεται και την υπηρεσία προϋποθέτει ότι όλα τα στοιχεία είναι μια συνεχόμενη μεταβλητή (δεν έλαβε δυνατότητες επιτρέπονται), ως την υπηρεσία μόνο εισόδων αριθμητικές τιμές την ώρα από τη δημιουργία της αυτήν την υπηρεσία web. Επίσης, η υπηρεσία αυτήν τη στιγμή χειρίζεται μέγεθος περιορισμένη δεδομένων, προθεσμία της φύσης αίτηση/απάντηση της κλήσης στην υπηρεσία web και το γεγονός ότι το μοντέλο είναι που χωρά κάθε φορά που ονομάζεται την υπηρεσία web. 

##<a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
Για συνήθεις ερωτήσεις για την κατανάλωση της υπηρεσίας web ή δημοσίευσης με το Azure Marketplace, ανατρέξτε στο θέμα [εδώ](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
