<properties 
    pageTitle="Τρόπος ρύθμισης των παραμέτρων τελικά σημεία Azure μηχανικής εκμάθησης στην ανάλυση ροή | Microsoft Azure" 
    description="Συναρτήσεις που ορίζονται από το χρήστη γλώσσας μηχανής στο ροή ανάλυσης"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>Ενοποίηση εκμάθησης στην ανάλυση ροή υπολογιστή

Ανάλυση ροή υποστηρίζει συναρτήσεις που ορίζονται από το χρήστη που κλήση για να τα τελικά σημεία Azure μηχανικής εκμάθησης. Υποστήριξη REST API για αυτή η δυνατότητα είναι λεπτομερή στη [βιβλιοθήκη ροή ανάλυση REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx). Σε αυτό το άρθρο παρέχει συμπληρωματικές πληροφορίες που χρειάζονται για την επιτυχή υλοποίηση από αυτήν τη δυνατότητα στην ανάλυση ροής. Ένα πρόγραμμα εκμάθησης έχει καταχωρηθεί επίσης και είναι διαθέσιμη [εδώ](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Επισκόπηση: Azure μηχανικής εκμάθησης ορολογία

Microsoft Azure μηχανικής εκμάθησης παρέχει ένα εργαλείο συνεργασίας, μεταφοράς και απόθεσης, μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε, δοκιμή και ανάπτυξη λύσεων πρόβλεψης ανάλυσης με τα δεδομένα σας. Αυτό το εργαλείο ονομάζεται το *Azure μηχανικής εκμάθησης Studio*. Το studio χρησιμοποιείται για αλληλεπίδραση με τους πόρους μηχανικής εκμάθησης και εύκολη δημιουργία, έλεγχος και διαδοχικές προσεγγίσεις σχετικά με το σχεδιασμό. Αυτοί οι πόροι και οι ορισμοί είναι κάτω από το στοιχείο.

- **Χώρος εργασίας**: το *χώρο εργασίας* είναι ένα κοντέινερ που περιέχει όλα τα άλλα μέσα μηχανικής εκμάθησης μαζί σε ένα κοντέινερ για τη διαχείριση και στοιχείο ελέγχου.
- **Έρευνα**: *δοκιμές* δημιουργούνται από επιστήμονες δεδομένων να χρησιμοποιούν σύνολα δεδομένων και να εκπαιδεύσετε μοντέλου μηχανικής εκμάθησης.
- **Τελικό σημείο**: *τα τελικά σημεία* είναι το αντικείμενο Azure μηχανικής εκμάθησης που χρησιμοποιείται για να τραβήξετε δυνατότητες ως είσοδο, εφαρμόστε ένα μοντέλο καθορισμένο μηχανικής εκμάθησης και επιστροφή έχει βαθμολογηθεί εξόδου.
- **Webservice βαθμολογίας**: μια *βαθμολογίας webservice* είναι μια συλλογή από τα τελικά σημεία που προαναφέρθηκαν.

Κάθε τελικό σημείο έχει apis για εκτέλεση ενεργειών δέσμης και σύγχρονη εκτέλεση. Ανάλυση ροή χρησιμοποιεί σύγχρονη εκτέλεση. Τη συγκεκριμένη υπηρεσία ονομάζεται [Υπηρεσία αίτηση/απάντηση](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) στο AzureML studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Μηχανικής εκμάθησης τους πόρους που απαιτούνται για την ανάλυση ροής εργασιών

Για τους σκοπούς της ανάλυσης ροή διεκπεραίωση των εργασιών, ένα τελικό σημείο αίτηση/απάντηση, μια [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)και έναν ορισμό swagger είναι όλα τα απαιτούμενα για επιτυχή εκτέλεση. Ανάλυση ροή έχει επιπλέον τελικού σημείου που κατασκευάζει τη διεύθυνση url για το τελικό swagger, αναζητά το περιβάλλον εργασίας και επιστρέφει έναν ορισμό UDF προεπιλογή για το χρήστη.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Ρύθμιση παραμέτρων μιας ροής ανάλυση και μηχανικής εκμάθησης UDF μέσω REST API

Χρησιμοποιώντας το ΥΠΌΛΟΙΠΟ APIs μπορείτε να ρυθμίσετε την εργασία σας για να καλέσετε λειτουργίες Azure μηχανικής γλώσσας. Τα βήματα είναι τα εξής:

1. Δημιουργία μιας εργασίας ροής ανάλυσης
2. Ορισμός μιας εισαγωγής
3. Ορισμός το αποτέλεσμα
4. Δημιουργία μιας συνάρτησης που ορίζονται από το χρήστη (UDF)
5. Συντάξτε ένα μετασχηματισμό ροή ανάλυσης που καλεί το UDF
6. Εκκίνηση της εργασίας

## <a name="creating-a-udf-with-basic-properties"></a>Δημιουργία μιας UDF με βασικές ιδιότητες

Ως παράδειγμα, ο ακόλουθος κώδικας δείγμα δημιουργεί μια ανυσματική UDF με το όνομα *newudf* που συνδέεται με ένα τελικό σημείο Azure μηχανικής εκμάθησης. Σημειώστε ότι μπορείτε να βρείτε το *τελικό σημείο* (υπηρεσία URI) στη σελίδα Βοήθειας API για την επιλεγμένη υπηρεσία και τα *apiKey* βρίσκονται στην κύρια σελίδα του υπηρεσίες.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Παράδειγμα σώμα αίτησης:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Τελικό σημείο RetrieveDefaultDefinition κλήσης για τις προεπιλεγμένες UDF

Όταν το σκελετό δημιουργηθεί UDF τον πλήρη ορισμό των το UDF είναι απαραίτητο. Το τελικό σημείο RetreiveDefaultDefinition σάς βοηθά να λάβετε τον ορισμό προεπιλογή για μια συνάρτηση ανυσμάτων που είναι δεσμευμένο με ένα τελικό σημείο Azure μηχανικής εκμάθησης. Το παρακάτω φορτίο απαιτεί να λάβετε τον ορισμό UDF προεπιλογή για μια συνάρτηση ανυσμάτων που είναι δεσμευμένο με ένα τελικό σημείο Azure μηχανικής εκμάθησης. Δεν μπορεί να καθορίσει την πραγματική τελικού σημείου όπως έχει ήδη παραχωρηθεί κατά τη διάρκεια αίτησης ΤΟΠΟΘΈΤΗΣΗ. Ανάλυση ροή καλεί το τελικό σημείο που παρέχεται στην πρόσκληση σε εάν παρέχεται ρητά. Διαφορετικά χρησιμοποιεί αυτήν αρχικά στα οποία γίνεται αναφορά. Εδώ θα πάρει η UDF μία συμβολοσειρά παραμέτρου (μια πρόταση) και επιστρέφει μία εξόδου του τύπου συμβολοσειράς που δείχνει την ετικέτα "άποψη" για αυτήν την πρόταση.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Παράδειγμα σώμα αίτησης:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Ένα δείγμα εξόδου της παρούσας αναζήτηση κάτι που θέλετε κάτω από.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>Ενημέρωση κώδικα UDF με την απόκριση 

Τώρα το UDF πρέπει να είναι ενημερωθεί με την προηγούμενη απόκριση, όπως φαίνεται παρακάτω.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Σώμα αίτησης (Έξοδος από το RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Υλοποίηση μετασχηματισμό ανάλυση ροής για να καλέσετε το UDF

Τώρα ερωτήματος το UDF (ονομάζονται εδώ scoreTweet) για κάθε συμβάν εισαγωγής και εγγραφής απόκριση για αυτό το συμβάν για το αποτέλεσμα.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Λήψη Βοήθειας
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ ανάλυση ροή Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)
