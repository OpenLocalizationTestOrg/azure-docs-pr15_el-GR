<properties 
    pageTitle="Ανάλυση επιβίωσης με Azure μηχανικής εκμάθησης | Microsoft Azure" 
    description="Πιθανότητα εμφάνισης συμβάν ανάλυσης επιβίωσης" 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>Ανάλυση επιβίωσης 

Στην περιοχή πολλά σενάρια, το κύριο αποτέλεσμα στην περιοχή αξιολόγησης είναι η ώρα σε ένα συμβάν που σας ενδιαφέρουν. Με άλλα λόγια, η ερώτηση "όταν αυτό το συμβάν θα παρουσιαστεί;" είναι σας ζητηθεί. Ως παραδείγματα, εξετάστε το ενδεχόμενο να περιπτώσεις όπου τα δεδομένα περιγράφει το χρόνο που πέρασε (ημέρες, έτη, απόστασης, κ.λπ.) μέχρι το συμβάν που σας ενδιαφέρουν (relapse ασθενείας, PhD βαθμού λάβατε, αποτυχία πληκτρολόγιο πέδης) πραγματοποιείται. Κάθε παρουσία των δεδομένων αντιπροσωπεύει ένα συγκεκριμένο αντικείμενο (μια υπομονή μαθητής, ένα αυτοκίνητο, κλπ.).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Αυτή η [υπηρεσία web]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) απαντήσεις στην ερώτηση "Τι είναι η πιθανότητα ότι θα συμβεί το συμβάν που σας ενδιαφέρουν με το χρόνο n για x? αντικειμένου" Με την παροχή ενός μοντέλου ανάλυσης επιβίωσης, αυτή η υπηρεσία web επιτρέπει στους χρήστες για την παροχή δεδομένων για να εκπαιδεύσετε το μοντέλο και να ελέγξετε. Το κύριο θέμα από την έρευνα είναι να του μοντέλου τη διάρκεια του χρόνου που πέρασε μέχρι το συμβάν που σας ενδιαφέρουν. 

>Αυτή η υπηρεσία web μπορεί να χρησιμοποιηθεί από χρήστες – ενδεχομένως μέσω της εφαρμογής για κινητές συσκευές, μέσω μιας τοποθεσίας Web ή ακόμα και σε έναν τοπικό υπολογιστή, για παράδειγμα. Αλλά ο σκοπός της υπηρεσίας web είναι επίσης για να λειτουργήσει ως παράδειγμα πώς Azure μηχανικής εκμάθησης μπορούν να χρησιμοποιηθούν για τη δημιουργία υπηρεσιών web επάνω σε κώδικα R. Με λίγα γραμμές κώδικα R και το κλικ ενός κουμπιού εντός Azure μηχανικής εκμάθησης Studio, μια έρευνα δυνατό να δημιουργηθούν με κωδικό R και δημοσιεύεται ως μια υπηρεσία web. Η υπηρεσία web μπορεί να, στη συνέχεια, δημοσιεύονται με το Azure Marketplace και που καταναλώνεται από τους χρήστες και τις συσκευές σε ολόκληρο τον κόσμο χωρίς ρύθμιση υποδομής από το συντάκτη της υπηρεσίας web.  

##<a name="consumption-of-web-service"></a>Κατανάλωση της υπηρεσίας web

Το σχήμα εισαγωγής δεδομένων της υπηρεσίας web εμφανίζονται στον παρακάτω πίνακα. Έξι τμήματα πληροφοριών απαιτούνται ως δεδομένα εισόδου: εκπαίδευση δεδομένων, έλεγχο των δεδομένων, ώρα που σας ενδιαφέρουν, το ευρετήριο "ώρα" διάσταση, το ευρετήριο "συμβάν" διάσταση και τους τύπους μεταβλητών (συνεχής ή παραγόντων). Τα δεδομένα του εκπαιδευτικού αναπαριστάται με μια συμβολοσειρά, όπου οι γραμμές διαχωρίζονται με κόμμα και τις στήλες είναι διαχωρισμένα με ελληνικό ερωτηματικό. Ο αριθμός των δυνατοτήτων των δεδομένων είναι ευέλικτη. Όλα τα στοιχεία στη συμβολοσειρά εισόδου πρέπει να είναι αριθμητική. Στα δεδομένα εκπαίδευση, τη διάσταση "ώρα" που υποδεικνύει τον αριθμό των μονάδων χρόνου (ημέρες, έτη, απόστασης, κ.λπ.) που έχουν παρέλθει από το σημείο έναρξης της μελέτης (μια υπομονή λαμβάνει προγράμματα επεξεργασίας ναρκωτικά, μελέτη PhD έναρξης μαθητές, αυτοκίνητο αρχίσουν να ελέγχεται, κ.λπ.) μέχρι το συμβάν που σας ενδιαφέρουν (το υπομονή επιστρέφοντας ναρκωτικά χρήση, το μαθητές απόκτηση βαθμού PhD, αυτοκίνητο αντιμετωπίζετε αποτυχία πέδης πληκτρολόγιο κ.λπ.) πραγματοποιείται. Τη διάσταση "συμβάν" υποδηλώνει εάν το συμβάν που σας ενδιαφέρουν εμφανίζεται στο τέλος της μελέτης. Η τιμή "συμβάν = 1" σημαίνει ότι το συμβάν που σας ενδιαφέρουν εμφανίζεται κατά τη στιγμή που υποδεικνύεται από τη διάσταση "ώρα"; "συμβάν = 0" σημαίνει ότι δεν εμφανίστηκε το συμβάν που σας ενδιαφέρουν από το χρόνο που υποδεικνύεται από τη διάσταση "ώρα".

- trainingdata - μια συμβολοσειρά χαρακτήρων. Οι γραμμές διαχωρίζονται με κόμμα και στήλες διαχωρίζονται με ελληνικό ερωτηματικό. Κάθε γραμμή περιλαμβάνει διάσταση "ώρα", "συμβάν" διάσταση και μεταβλητές πρόβλεψης.
- testingdata - μία γραμμή δεδομένων που περιέχει μεταβλητές πρόβλεψης για ένα συγκεκριμένο αντικείμενο.
- time_of_interest - ο χρόνος που πέρασε του n που σας ενδιαφέρουν.
- index_time - το ευρετήριο στήλης τη διάσταση "ώρα" (ξεκινώντας από το 1).
- index_event - το ευρετήριο στήλης τη διάσταση "συμβάν" (ξεκινώντας από το 1).
- variable_types - μιας συμβολοσειράς χαρακτήρων με ελληνικά ερωτηματικά ως διαχωριστικά σε αυτό. συνεχής μεταβλητές αντιπροσωπεύει το 0 και 1 αντιπροσωπεύει το συντελεστή μεταβλητές.


Το αποτέλεσμα είναι η πιθανότητα ενός συμβάντος που συμβαίνουν σε συγκεκριμένο χρόνο. 

>Αυτή η υπηρεσία, όπως φιλοξενούνται σε το Azure Marketplace, είναι μια υπηρεσία OData. αυτές μπορεί να ονομάζεται μέσω ΔΗΜΟΣΊΕΥΣΗ ή ΛΉΨΗ μεθόδους. 

Υπάρχουν πολλοί τρόποι του κατανάλωση η υπηρεσία με ένα αυτοματοποιημένο τρόπο (είναι μια εφαρμογή παράδειγμα [εδώ](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Έναρξη κώδικα C# για κατανάλωση υπηρεσίας web:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




Την ερμηνεία των αυτόν τον έλεγχο είναι ως εξής. Υπό την προϋπόθεση ότι ο στόχος των δεδομένων είναι να του μοντέλου ο χρόνος που πέρασε μέχρι την επιστροφή σε χρήση ναρκωτικά για ασθενείς που λάβει ένα από τα προγράμματα δύο επεξεργασίας. Το αποτέλεσμα του διαβάζει υπηρεσίας web: για ασθενών που 35 ετών, χρειάζεται προηγούμενη λαθρεμπορίου επεξεργασίας 2 επί, λαμβάνοντας το πρόγραμμα επεξεργασίας χρόνο κατοικιών, με χρήση heroin και cocaine, την πιθανότητα επιστρέφει τη χρήση της είναι 95.64% κατά ημέρα 500.

##<a name="creation-of-web-service"></a>Δημιουργία της υπηρεσίας web

>Αυτή η υπηρεσία web που δημιουργήθηκε με χρήση Azure μηχανικής εκμάθησης. Για μια δωρεάν δοκιμαστική έκδοση, καθώς και εισαγωγικά βίντεο σχετικά με τη δημιουργία δοκιμών και [δημοσίευσης υπηρεσίες web](machine-learning-publish-a-machine-learning-web-service.md), ανατρέξτε στο θέμα [azure.com/ml](http://azure.com/ml). Ακολουθεί ένα στιγμιότυπο οθόνης από την έρευνα που δημιουργήσατε τον κώδικα υπηρεσίας και παράδειγμα web για κάθε μία από τις λειτουργικές μονάδες μέσα την έρευνα.

Από μέσα σε Azure μηχανικής εκμάθησης, μια νέα κενή έρευνας δημιουργήθηκε και δύο [Εκτέλεση δέσμης ενεργειών R] [ execute-r-script] λειτουργικές μονάδες έχουν τα μηνύματα μεταφέρονται σε χώρο εργασίας. Το σχήμα των δεδομένων που δημιουργήθηκε με μια απλή [Δέσμη ενεργειών εκτέλεση R][execute-r-script], που ορίζει το σχήμα δεδομένων εισόδου για την υπηρεσία web. Η ενότητα αυτή, στη συνέχεια, συνδέεται με το δεύτερο [Εκτέλεση δέσμης ενεργειών R] [ execute-r-script] λειτουργική μονάδα, των κύριων εργασίας. Η ενότητα αυτή η προ-επεξεργασία αρχείου δεδομένων μοντέλου δόμησης και προβλέψεων. Στο βήμα προεπεξεργασίας δεδομένων, τα δεδομένα εισόδου που αντιπροσωπεύονται από μια μεγάλη συμβολοσειρά μετατρέπονται και που έχει μετατραπεί σε ένα πλαίσιο δεδομένων. Στο βήμα δόμησης μοντέλου, πακέτου εξωτερικών R "survival_2.37-7.zip" είναι εγκατεστημένη πρώτα για τη διεξαγωγή επιβίωσης ανάλυσης. Στη συνέχεια, η συνάρτηση "coxph" εκτελείται μετά από μια σειρά εργασίες επεξεργασίας δεδομένων. Μπορείτε να διαβάσετε τις λεπτομέρειες της συνάρτησης "coxph" για την ανάλυση επιβίωσης από την τεκμηρίωση R. Στο βήμα πρόβλεψη, μια παρουσία δοκιμών παρέχεται στο μοντέλο εκπαιδευμένο με τη συνάρτηση "surfit" και την καμπύλη επιβίωσης για αυτήν την παρουσία δοκιμών παράγεται ως μεταβλητή "Καμπύλη". Τέλος, λαμβάνεται η πιθανότητα το χρόνο που σας ενδιαφέρουν. 

###<a name="experiment-flow"></a>Ροή έρευνας:

![Πειραματιστείτε ροής][1]

####<a name="module-1"></a>Λειτουργική μονάδα 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>Λειτουργική μονάδα 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Περιορισμοί

Αυτή η υπηρεσία web μπορεί να διαρκέσει μόνο αριθμητικές τιμές ως δυνατότητα μεταβλητές (στήλες). Η στήλη "συμβάν" μπορεί να διαρκέσει μόνο τιμή 0 ή 1. Η στήλη "ώρα" πρέπει να είναι θετικός ακέραιος.

##<a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
Για συνήθεις ερωτήσεις για την κατανάλωση της υπηρεσίας web ή δημοσίευσης με το Azure Marketplace, ανατρέξτε στο θέμα [εδώ](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
