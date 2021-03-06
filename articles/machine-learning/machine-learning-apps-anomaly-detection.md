<properties 
    pageTitle="Εφαρμογή μηχανικής εκμάθησης: ανωμαλία εντοπισμού υπηρεσιών | Microsoft Azure" 
    description="API εντοπισμού ανωμαλία είναι ένα παράδειγμα δημιουργηθεί με το Microsoft Azure μηχανικής εκμάθησης που εντοπίζει ανωμαλίες σε ώρα σειρές δεδομένων με αριθμητικές τιμές που είναι ομοιόμορφα, σε χρόνο." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Υπηρεσία εντοπισμού ανωμαλία εκμάθησης υπολογιστή#

##<a name="overview"></a>Επισκόπηση

[API εντοπισμού ανωμαλία](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) είναι ένα παράδειγμα ενσωματωμένο με το Azure μηχανικής εκμάθησης που εντοπίζει ανωμαλίες σε ώρα σειρές δεδομένων με αριθμητικές τιμές που είναι ομοιόμορφα, σε χρόνο. 

Αυτό το API μπορεί να εντοπίσει τους ακόλουθους τύπους ύπαρξη μοτίβα στο χρόνο σειρές δεδομένων:

* **Θετικές και αρνητικές τάσεις**: για παράδειγμα, όταν παρακολούθηση χρήσης μνήμης σε υπολογιστική μια τάση προς τα επάνω ενδέχεται να σας ενδιαφέρουν όπως αυτό μπορεί να σημαίνει απώλεια μνήμης,

* **Αλλαγές σε το δυναμικό εύρος των τιμών**: για παράδειγμα, κατά την παρακολούθηση του εξαιρέσεις που ανακύπτουν από μια υπηρεσία cloud, τυχόν αλλαγές στο το δυναμικό εύρος των τιμών μπορεί να υποδεικνύουν αστάθεια στο την εύρυθμη λειτουργία της υπηρεσίας, και

* **Φθάσει και Dips**: για παράδειγμα, κατά την παρακολούθηση ο αριθμός αποτυχιών σύνδεσης σε μια υπηρεσία ή τον αριθμό ανάληψης ελέγχου σε μια τοποθεσία ηλεκτρονικού εμπορίου, αιχμές ή πτώσεις μπορεί να υποδεικνύουν αφύσικη συμπεριφορά.

Αυτές οι ανιχνευτές μηχανικής εκμάθησης παρακολουθούν τέτοιου είδους αλλαγές στις τιμές ώρας και αναφορά σε εξέλιξη αλλαγές στο τις τιμές τους ως βαθμολογίες ανωμαλία. Δεν απαιτείται ad-hoc στο όριο της ρύθμισης και των βαθμών που μπορεί να χρησιμοποιηθεί για τον έλεγχο της ψευδές θετικό ποσοστό. Ο εντοπισμός ανωμαλία API είναι χρήσιμη σε διάφορες σενάρια όπως υπηρεσία παρακολούθησης, παρακολουθώντας KPI διάρκεια του χρόνου, χρήση παρακολούθηση μέσω μετρικά όπως τον αριθμό των αναζητήσεις, αριθμούς μόνο κλικ, παρακολούθηση των επιδόσεων μέσω μετρητές όπως μνήμη, CPU, διαβάζει το αρχείο, κ.λπ. μέσα στο χρόνο.

Η προσφορά εντοπισμού ανωμαλία διατίθεται με χρήσιμα εργαλεία για να ξεκινήσετε. 

* Η [εφαρμογή web](http://anomalydetection-aml.azurewebsites.net/) σάς βοηθά να αξιολογήσετε και να οπτικοποιήσετε τα αποτελέσματα της ανίχνευσης ανωμαλία APIs στα δεδομένα σας. 

* Το [δείγμα κώδικα](http://adresultparser.codeplex.com/) δείχνει τον τρόπο πρόσβασης το API και ανάλυση τα αποτελέσματα σε C# μέσω προγράμματος.

>[AZURE.NOTE]
>Δοκιμάστε **λύσης ΠΛΗΡΟΦΟΡΙΚΉΣ ανωμαλία ιδέες** υποστηρίζεται από [αυτό το API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection)
>
>Για να λάβετε αυτήν τη λύση τελικών αναπτυχθεί στη συνδρομή σας στο Azure <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **Ξεκινήστε εδώ >**</a>


##<a name="api-definition"></a>Ορισμός API

Η υπηρεσία παρέχει ένα βάσει REST API μέσω HTTPS που μπορεί να χρησιμοποιηθεί με διαφορετικούς τρόπους, όπως μια web ή μια εφαρμογή κινητές συσκευές, R, Python, Excel, κ.λπ.  Αποστολή δεδομένων σειράς ώρας σε αυτήν την υπηρεσία μέσω κλήσης REST API και εκτελεί ένας συνδυασμός από τους τρεις ανωμαλία τύπους που περιγράφονται παραπάνω. Η υπηρεσία εκτελείται την πλατφόρμα Azure μηχανικής εκμάθησης, η οποία κλίμακες για τις ανάγκες σας επιχειρήσεις απρόσκοπτα και παρέχει SLA.

###<a name="headers"></a>Κεφαλίδες
Βεβαιωθείτε ότι συμπεριλαμβάνετε τη σωστή κεφαλίδες στην πρόσκληση σε, που πρέπει να είναι ως εξής:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Μπορείτε να βρείτε το κλειδί λογαριασμού σας από το λογαριασμό σας στην [Αγορά δεδομένων Azure](https://datamarket.azure.com/account/keys). 

###<a name="score-api"></a>API βαθμολογία

Το API βαθμολογία χρησιμοποιείται για την εκτέλεση ανίχνευσης ανωμαλία σε μη εποχιακά ώρα σειράς δεδομένων. Το API εκτελεί ένας αριθμός ανιχνευτές ανωμαλία στα δεδομένα και επιστρέφει τις βαθμολογίες ανωμαλία. Η παρακάτω εικόνα εμφανίζει ένα παράδειγμα ανωμαλίες που μπορούν να εντοπίσουν το API βαθμολογία. Αυτό χρονολογική σειρά έχει 2 σημαντικές αλλαγές σε επίπεδο και 3 αιχμές. Το κόκκινο τελείες εμφανίζουν την ώρα κατά την οποία εντοπιστεί η αλλαγή του επιπέδου, ενώ το μαύρες κουκκίδες εμφανίζουν τις αιχμές εντοπίστηκε.

![API βαθμολογία][1]
    
**ΔΙΕΎΘΥΝΣΗ URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Παράδειγμα σώμα αίτησης**

Στην πρόσκληση σε παρακάτω, στείλουμε ένα χρονολογική σειρά (απεικονίζεται έχει περικοπεί) με σημεία δεδομένων από το 1/3/2016 έως 10/3/2016 και τις παραμέτρους των ανιχνευτών Συλλέκτη για το API για τον εντοπισμό εάν οποιοδήποτε από αυτά τα σημεία δεδομένων είναι ύπαρξη. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

Απάντηση σε αυτό θα είναι: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality API

Το API ScoreWithSeasonality χρησιμοποιείται για την εκτέλεση ανίχνευσης ανωμαλία σε χρονολογική σειρά που έχουν εποχιακά μοτίβα. Αυτό το API είναι χρήσιμη για τον εντοπισμό αποκλίσεων σε εποχιακά μοτίβα.  

Η παρακάτω εικόνα εμφανίζει ένα παράδειγμα ανωμαλίες εντοπίζονται σε μια σειρά εποχιακά ώρα. Η χρονολογική σειρά έχει μία Συλλέκτη (το 1ος μαύρη κουκκίδα), δύο πτώσεις (η 2η μαύρη κουκκίδα και στο τέλος) και μία Αλλαγή επιπέδου (κόκκινη κουκκίδα). Σημειώστε ότι τόσο το dip στο μέσο της σειράς ώρας και η αλλαγή του επιπέδου μόνο γεμίσματος αφού εποχιακά στοιχεία έχουν καταργηθεί από τη σειρά.

![Εποχικότητα API][2]

**ΔΙΕΎΘΥΝΣΗ URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Παράδειγμα σώμα αίτησης**

Στην πρόσκληση σε παρακάτω, στείλουμε ένα χρονολογική σειρά (απεικονίζεται έχει περικοπεί) με σημεία δεδομένων από το 1/3/2016 έως 10/3/2016 και τις παραμέτρους για το API για τον εντοπισμό εάν οποιοδήποτε από αυτά τα σημεία δεδομένων είναι ύπαρξη. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

Απάντηση σε αυτό θα είναι: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Ανιχνευτές

Ο εντοπισμός ανωμαλία API υποστηρίζει ανιχνευτές σε 3 ευρείες κατηγορίες. Μπορείτε να βρείτε λεπτομέρειες σχετικά με συγκεκριμένες παραμέτρους εισόδου και εξόδους για κάθε πρόγραμμα ανίχνευσης στον παρακάτω πίνακα.

|Κατηγορία ανίχνευσης|Πρόγραμμα ανίχνευσης|Περιγραφή|Παράμετροι εισόδου|Εξόδους
|---|---|---|---|---|
|Ανιχνευτές Συλλέκτη|Πρόγραμμα ανίχνευσης TSpike|Εντοπισμός αιχμές και πτώσεις βάσει άκρο των τιμών είναι από το πρώτο και το τρίτο τεταρτημόρια|*tspikedetector.sensitivity:* λαμβάνει ακέραια τιμή στην περιοχή προεπιλεγμένη 1-10: 3; Υψηλότερες τιμές θα ενημερωθείτε περισσότερες ακραίες τιμές, επομένως, καθιστώντας λιγότερο ευαίσθητες|TSpike: δυαδικών τιμών – '1' σε περίπτωση που εντοπιστεί ένα Συλλέκτη/dip, διαφορετικά ' 0'|
||Πρόγραμμα ανίχνευσης ZSpike|Εντοπισμός αιχμές και πτώσεις βάση πόσο μακριά είναι το datapoints από τον μέσο τους|*zspikedetector.sensitivity:* λαμβάνουν ακέραια τιμή στην περιοχή προεπιλεγμένη 1-10: 3; Υψηλότερες τιμές θα ενημερωθείτε περισσότερες ακραίες τιμές, καθιστώντας λιγότερο ευαίσθητες|ZSpike: δυαδικών τιμών – '1' σε περίπτωση που εντοπιστεί ένα Συλλέκτη/dip, διαφορετικά ' 0'|
|Πρόγραμμα ανίχνευσης αργή τάσης|Πρόγραμμα ανίχνευσης αργή τάσης|Εντοπισμός αργή θετική τάση σύμφωνα με την ευαισθησία του συνόλου|*trenddetector.sensitivity:* όριο στη βαθμολογία ανίχνευσης (προεπιλογή: 3,25, 3,25 – 5 είναι λογικό περιοχή για να επιλέξετε αυτό από; Όσο υψηλότερη το λιγότερο πεζών-κεφαλαίων)|TScore: αριθμός κινητής που αντιπροσωπεύει ανωμαλία βαθμολογία στην τάσης|
|Αλλαγή του επιπέδου ανιχνευτές|Αλλαγή του επιπέδου μονής ανίχνευσης|Εντοπισμός προς τα επάνω επίπεδο αλλαγή σύμφωνα με την ευαισθησία του συνόλου|*upleveldetector.sensitivity:* όριο στη βαθμολογία ανίχνευσης (προεπιλογή: 3,25, 3,25 – 5 είναι λογικό περιοχή για να επιλέξετε αυτό από; Όσο υψηλότερη το λιγότερο πεζών-κεφαλαίων)|PScore: αριθμός που αντιπροσωπεύει ανωμαλία βαθμολογία στην προς τα επάνω κινητής Αλλαγή επιπέδου|
||Πρόγραμμα ανίχνευσης Αλλαγή επιπέδου διπλής κατεύθυνσης|Εντοπισμός και προς τα επάνω και προς τα κάτω αλλαγή του επιπέδου σύμφωνα με την ευαισθησία του συνόλου|*bileveldetector.sensitivity:* όριο στη βαθμολογία ανίχνευσης (προεπιλογή: 3,25, 3,25 – 5 είναι λογικό περιοχή για να επιλέξετε αυτό από; Όσο υψηλότερη το λιγότερο πεζών-κεφαλαίων)|RPScore: αριθμός που αντιπροσωπεύουν ανωμαλία βαθμολογία στην προς τα επάνω και προς τα κάτω επίπεδο αλλάζουν αιωρούμενα

###<a name="parameters"></a>Παράμετροι

Πιο λεπτομερείς πληροφορίες σχετικά με αυτές τις παραμέτρους εισόδου είναι που παρατίθενται στον παρακάτω πίνακα:

|Παράμετροι εισόδου|Περιγραφή|Προεπιλεγμένη ρύθμιση|Τύπος|Έγκυρη περιοχή|Προτεινόμενη περιοχή|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Συνάθροιση διαστήματος σε δευτερόλεπτα για συνάθροιση εισαγωγής χρονολογική σειρά|0 (δεν έχει εκτελεστεί συνάθροιση)|Ακέραιος αριθμός|0: παραλείψετε διαφορετικά συνάθροιση, > 0|5 λεπτά για 1 ημέρα, ανάλογα με χρονολογική σειρά
|preprocess.aggregationFunc|Συνάρτηση που χρησιμοποιείται για συγκέντρωση δεδομένων σε το καθορισμένο AggregationInterval|μέση τιμή|Απαρίθμηση|Μέση, sum, μήκους|Δ/Υ|
|preprocess.replaceMissing|Τιμές που χρησιμοποιούνται τεκμαρτό δεδομένα που λείπουν|lkv (γνωστό τελευταία τιμή)|Απαρίθμηση|μηδέν, lkv, τον μέσο|Δ/Υ|
|detectors.historyWindow|Ιστορικό (σε # των σημείων δεδομένων) χρησιμοποιείται για υπολογισμούς βαθμολογία ανωμαλία|500|Ακέραιος αριθμός|10-2000|Εξαρτάται από το χρονολογική σειρά|
|upleveldetector.sensitivity|Διαβάθμιση για προς τα επάνω επίπεδο αλλάξετε πρόγραμμα ανίχνευσης. |3,25|διπλά|Κανένας|3,25 5(Lesser values mean more sensitive)|
|bileveldetector.sensitivity|Διαβάθμιση για το επίπεδο διπλής κατεύθυνσης αλλάξετε πρόγραμμα ανίχνευσης. |3,25|διπλά|Κανένας|3,25 5(Lesser values mean more sensitive)|
|trenddetector.sensitivity|Διαβάθμιση για το πρόγραμμα ανίχνευσης θετική τάση. |3,25|διπλά|Κανένας|3,25 5(Lesser values mean more sensitive)|
|tspikedetector.sensitivity |Διαβάθμιση για TSpike ανίχνευσης|3|Ακέραιος αριθμός|1-10|3-5(Lesser values mean more sensitive)|
|zspikedetector.sensitivity |Διαβάθμιση για ZSpike ανίχνευσης|3|Ακέραιος αριθμός|1-10 |3-5(Lesser values mean more sensitive)|
|seasonality.Enable|Εάν η ανάλυση εποχικότητα είναι να εκτελεστεί|TRUE|δυαδική τιμή|τιμή TRUE, false|Εξαρτάται από το χρονολογική σειρά|
|seasonality.numSeasonality |Μέγιστος αριθμός περιοδικό κύκλους για να εντοπίζονται|1|Ακέραιος αριθμός|1, 2|1-2|
|seasonality.Transform |Εάν εποχιακά (και) στοιχεία τάσης αφαιρούνται πριν από την εφαρμογή ανωμαλία ανίχνευσης|deseason|Απαρίθμηση|κανένα, deseason, deseasontrend|Δ/Υ|
|postprocess.tailRows |Αριθμός των σημείων δεδομένων πιο πρόσφατη να διατηρηθούν στα αποτελέσματα εξόδου|0|Ακέραιος αριθμός|0 (διατηρήσετε όλα τα σημεία δεδομένων), ή να καθορίσετε το πλήθος των σημείων για να διατηρήσετε στα αποτελέσματα|Δ/Υ|

###<a name="output"></a>Εξόδου
Το API εκτελείται όλοι οι ανιχνευτές στα δεδομένα σας σειρά ώρα και επιστρέφει βαθμολογίες ανωμαλία και δείκτες δυαδικό Συλλέκτη για κάθε σημείο στο χρόνο. Ο παρακάτω πίνακας παραθέτει εξόδους από το API. 

|Εξόδους|Περιγραφή|
|---|---|
|Ώρα|Χρονικές σημάνσεις από ανεπεξέργαστα δεδομένα ή το πλήθος των συγκεντρωτικών αποτελεσμάτων (ή/και) τεκμαρτές δεδομένων εάν συνάθροισης (ή/και) λείπουν εφαρμόζεται τον υπολογισμό των τεκμαρτών δεδομένων|
|OriginalData|Τις τιμές από ανεπεξέργαστα δεδομένα ή το πλήθος των συγκεντρωτικών αποτελεσμάτων (ή/και) τεκμαρτές δεδομένων εάν συνάθροισης (ή/και) λείπουν εφαρμόζεται τον υπολογισμό των τεκμαρτών δεδομένων|
|ProcessedData|Ένα από τα εξής: <ul><li>Προσαρμοσμένο εποχιακά χρονολογική σειρά εάν σημαντική εποχικότητα Εντοπίστηκε και deseason επιλογή;</li><li>εποχιακά προσαρμοστεί και detrended χρονολογική σειρά εάν εντοπίστηκε σημαντική εποχικότητα και deseasontrend επιλογή</li><li>Διαφορετικά, αυτή είναι η ίδια με OriginalData</li>|
|TSpike|Δυαδικό ένδειξη για να υποδείξετε εάν μια Συλλέκτη εντοπίζεται από το πρόγραμμα ανίχνευσης TSpike|
|ZSpike|Δυαδικό ένδειξη για να υποδείξετε εάν μια Συλλέκτη εντοπίζεται από το πρόγραμμα ανίχνευσης ZSpike|
|Pscore|Ένας αιωρούμενη αριθμός που αντιπροσωπεύει βαθμολογία ανωμαλία προς τα επάνω επίπεδο αλλαγή|
|Palert|η τιμή 1/0 που υποδεικνύει ότι υπάρχει ένα επίπεδο προς τα επάνω αλλαγή ανωμαλία με βάση την ευαισθησία εισαγωγής|
|RPScore|Μια αιωρούμενη αριθμών που αντιπροσωπεύουν ανωμαλία βαθμολογία στην αλλαγή του επιπέδου διπλής κατεύθυνσης|
|RPAlert|η τιμή 1/0 που υποδεικνύει ότι υπάρχει ένα επίπεδο διπλής κατεύθυνσης αλλαγή ανωμαλία με βάση την ευαισθησία εισαγωγής|
|TScore|Μια αιωρούμενη αριθμών που αντιπροσωπεύουν ανωμαλία βαθμολογία σε θετικό τάσης|
|TAlert|1/0 τιμή που υποδεικνύει ότι είναι μια ανωμαλία θετική τάση με βάση την ευαισθησία εισαγωγής|


Αυτό το αποτέλεσμα μπορεί να αναλυθεί χρησιμοποιώντας μια [απλή ανάλυσης](https://adresultparser.codeplex.com/) - έχει δείγμα κώδικα που δείχνει πώς μπορείτε να συνδεθείτε με το API και να αναλύσετε το αποτέλεσμα. Το ανωμαλίες εντοπίζονται είναι δυνατό να απεικονιστούν σε έναν πίνακα εργαλείων ή/και αντανάκλαση, έως human ειδικούς για διορθωτικές ενέργειες ή ενσωματωμένο στο έκδοσης εισιτηρίων συστήματα.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
