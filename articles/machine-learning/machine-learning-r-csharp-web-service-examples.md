<properties 
    pageTitle="Παραδείγματα που έχουν δημιουργηθεί με R των υπηρεσιών μηχανικής εκμάθησης web | Microsoft Azure" 
    description="Βρείτε ένα σύνολο χρήσιμες web παραδείγματα υπηρεσιών που δημιουργήθηκαν με κωδικό R και μηχανικής εκμάθησης και δημοσιεύονται το Azure Marketplace." 
    keywords="csharp, r κώδικα, παραδείγματα υπηρεσιών web"
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


#<a name="web-services-examples-using-r-code-on-azure-machine-learning-and-published-to-microsoft-azure-marketplace"></a>Web των υπηρεσιών παραδείγματα με χρήση κώδικα R σε Azure μηχανικής εκμάθησης και δημοσιευτεί στο Microsoft Azure Marketplace

Σε αυτό το άρθρο είναι παράδειγμα web υπηρεσίες δημιουργήθηκαν χρησιμοποιώντας Azure μηχανικής εκμάθησης και δημοσιεύονται το Azure Marketplace. Κάθε παράδειγμα υπηρεσίας web περιλαμβάνει μια ολοκληρωμένη συνημμένο έγγραφο, η ενσωμάτωση σύνολα δεδομένων δείγματος για τον έλεγχο των υπηρεσιών και εξηγεί πώς ο χρήστης μπορεί να δημιουργήσει μια παρόμοια υπηρεσία τον εαυτό τους. 

Στο Azure μηχανικής εκμάθησης Studio, οι χρήστες μπορούν να σύνταξη κώδικα R και με λίγα μόνο κλικ, δημοσιεύστε το ως υπηρεσία web για εφαρμογές και συσκευές για την εκμετάλλευση όλο τον κόσμο. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Δημιουργία απλού υπολογισμού που παρέχουν στατιστικές λειτουργικότητα για τη δημιουργία ενός προσαρμοσμένου πρόβλεψης ανάλυσης άποψη εξόρυξης κειμένου, από νέα και έμπειρους χρήστες R μπορούν να επωφεληθούν από την ευκολία με τον οποίο οι χρήστες του Azure μηχανικής εκμάθησης να operationalize R κώδικα. Ενώ είναι αυτές τις υπηρεσίες μπορεί να χρησιμοποιηθεί από χρήστες, ενδεχομένως μέσω της εφαρμογής για κινητές συσκευές ή μια τοποθεσία Web, ο σκοπός της αυτά τα παραδείγματα υπηρεσιών web web για να εμφανίσετε τον τρόπο να operationalize μηχανικής εκμάθησης R δέσμες ενεργειών για σκοπούς ανάλυσης και να χρησιμοποιηθεί για τη δημιουργία υπηρεσιών web επάνω σε κώδικα R.

Παράδειγμα κάθε περιλαμβάνει ένα παράδειγμα C# για κατανάλωση υπηρεσία web.


![Διάγραμμα R κώδικα σε Azure μηχανικής εκμάθησης: R λύσεων για κοινή χρήση ή δημοσίευση για να το Azure Marketplace.][1]

Λάβετε υπόψη τα παρακάτω σενάρια.

##<a name="scenario-1-generic-model"></a>Σενάριο 1: Μοντέλο γενικής χρήσης 
Ένας χρήστης εργάζεται με ένα μοντέλο γενικής χρήσης που μπορούν να εφαρμοστούν σε έναν νέο χρήστη δεδομένων, όπως μια βασική προβλέψεις ώρα σειρά δεδομένων ή συγκεκριμένου μέθοδο R με προηγμένη ανάλυση. Αυτός ο χρήστης δημοσιεύει το μοντέλο ως υπηρεσία web για άλλους για την εκμετάλλευση με τα δεδομένα τους.



* [Δυαδικό τάξης](machine-learning-r-csharp-binary-classifier.md)
* [Μοντέλο συμπλέγματος](machine-learning-r-csharp-cluster-model.md)
* [Multivariate γραμμικής παλινδρόμησης](machine-learning-r-csharp-multivariate-linear-regression.md)
* [Προβλέψεις - Εκθετική εξομάλυνση](machine-learning-r-csharp-forecasting-exponential-smoothing.md)
* [ETS + STL προβλέψεις](machine-learning-r-csharp-retail-demand-forecasting.md)
* [Προβλέψεις - Autoregressive ολοκλήρωμα μετακίνηση μέσος όρος (ARIMA)](machine-learning-r-csharp-arima.md)
* [Ανάλυση επιβίωσης](machine-learning-r-csharp-survival-analysis.md)


##<a name="scenario-2-trained-model--specific-data"></a>Σενάριο 2: Εκπαιδευμένο μοντέλο – συγκεκριμένων δεδομένων 
Ένας χρήστης έχει δεδομένα που παρέχει χρήσιμες προβλέψεων μέσω R κώδικα, όπως ένα μεγάλο δείγμα των ομαδοποιημένων μέσω έναν αλγόριθμο k σημαίνει πρόβλεψη τύπος προσωπικότητα του χρήστη ή δεδομένα έρευνα εύρυθμης λειτουργίας που μπορεί να χρησιμοποιηθεί για την πρόβλεψη κινδύνου ενός ατόμου για καρκίνο των πνευμόνων με ένα πακέτο ανάλυσης R επιβίωσης ερωτηματολογίων προσωπικότητα. Ο χρήστης δημοσιεύει τα δεδομένα μέσω της υπηρεσίας web που προβλέπει αποτέλεσμα έναν νέο χρήστη.

##<a name="scenario-3-trained-model--generic-data"></a>Σενάριο 3: Εκπαιδευμένο μοντέλο – γενικής χρήσης δεδομένων 
Ένας χρήστης έχει γενικής χρήσης δεδομένων (όπως μια σώματος κειμένου) που επιτρέπει σε μια υπηρεσία web για να δημιουργηθεί και να εφαρμοστεί γενικά σε διαφορετικούς τύπους περιπτώσεις χρήσης και σενάρια.

* [Λεξικό βάσει άποψη ανάλυσης](machine-learning-r-csharp-lexicon-based-sentiment-analysis.md)

##<a name="scenario-4-advanced-calculator"></a>Σενάριο 4: Αριθμομηχανής για προχωρημένους 
Ένας χρήστης παρέχει σύνθετων υπολογισμών ή προσομοιώσεις που δεν απαιτούν οποιαδήποτε εκπαιδευμένο μοντέλο ή προσαρμογή ενός μοντέλου με τα δεδομένα του χρήστη.

* [Διαφορά στο αναλογίες δοκιμής](machine-learning-r-csharp-difference-in-two-proportions.md)
* [Κανονική κατανομή οικογένεια προγραμμάτων](machine-learning-r-csharp-normal-distribution.md)
* [Οικογένεια διωνυμική κατανομή](machine-learning-r-csharp-binomial-distribution.md)

##<a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
Για συνήθεις ερωτήσεις για την κατανάλωση της υπηρεσίας web ή δημοσίευση στο Store, ανατρέξτε στο θέμα [εδώ](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-web-service-examples/machine-learning-r-code-options-for-using-and-sharing-cloud.png


 
