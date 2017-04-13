<properties 
    pageTitle="Εκτέλεση δέσμης ενεργειών U-SQL στην ανάλυση λίμνης Azure δεδομένων από εργοστασίου δεδομένων Azure" 
    description="Μάθετε πώς να επεξεργάζονται δεδομένα, εκτελώντας δέσμες ενεργειών U SQL Azure δεδομένων λίμνης ανάλυση υπολογισμού την υπηρεσία." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>Εκτέλεση δέσμης ενεργειών U-SQL στην ανάλυση λίμνης Azure δεδομένων από εργοστασίου δεδομένων Azure
> [AZURE.SELECTOR]
[Ομάδα](data-factory-hive-activity.md)  
[Γουρούνι](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Ροή Hadoop](data-factory-hadoop-streaming-activity.md)
[Μηχανικής εκμάθησης](data-factory-azure-ml-batch-execution-activity.md) 
[Αποθηκευμένη διαδικασία](data-factory-stored-proc-activity.md)
[U-SQL ανάλυσης δεδομένων λίμνης](data-factory-usql-activity.md)
[.NET προσαρμοσμένο](data-factory-use-custom-activities.md)
 
Μια διαδικασία σε μια προέλευση δεδομένων Azure επεξεργάζεται τα δεδομένα στις υπηρεσίες συνδεδεμένων χώρου αποθήκευσης με χρήση συνδεδεμένων υπολογισμού των υπηρεσιών. Περιέχει μια ακολουθία δραστηριοτήτων όπου κάθε δραστηριότητα εκτελεί μια συγκεκριμένη επεξεργασίας. Σε αυτό το άρθρο περιγράφει τη **Δραστηριότητα U-SQL ανάλυση λίμνης δεδομένων** που εκτελεί μια δέσμη ενεργειών **U-SQL** σε μια υπηρεσία υπολογισμού συνδεδεμένες **Azure δεδομένων λίμνης ανάλυσης** . 

> [AZURE.NOTE] 
> Δημιουργήστε ένα λογαριασμό ανάλυση λίμνης Azure δεδομένων πριν από τη δημιουργία μιας διοχέτευσης με μια δραστηριότητα δεδομένων λίμνης ανάλυση U-SQL. Για να μάθετε σχετικά με την ανάλυση λίμνης Azure δεδομένων, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυσης](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
>  
> Αναθεώρηση [Δημιουργήστε το πρώτο πρόγραμμα εκμάθησης διαδικασία](data-factory-build-your-first-pipeline.md) για λεπτομερή βήματα για να δημιουργήσετε μια προέλευση δεδομένων, συνδεδεμένες υπηρεσίες, σύνολα δεδομένων και μια διαδικασία. Χρησιμοποιήστε τμήματα κώδικα JSON με το πρόγραμμα επεξεργασίας εργοστασίου δεδομένων ή Visual Studio ή Azure PowerShell για τη δημιουργία οντοτήτων εργοστασίου δεδομένων.

## <a name="azure-data-lake-analytics-linked-service"></a>Ανάλυση λίμνης Azure δεδομένων συνδεδεμένες υπηρεσίας
Μπορείτε να δημιουργήσετε μια υπηρεσία **Ανάλυσης λίμνης δεδομένων Azure** συνδεδεμένες για να συνδέσετε μια υπηρεσία υπολογισμού Azure δεδομένων λίμνης ανάλυσης με μια εργοστασίου Azure δεδομένων. Η δραστηριότητα U-SQL ανάλυσης δεδομένων λίμνης στη διοχέτευση αναφέρεται σε αυτήν την υπηρεσία συνδεδεμένων. 

Το παρακάτω παράδειγμα παρέχει ορισμό JSON για μια υπηρεσία Azure δεδομένων λίμνης Analytics συνδεδεμένες. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


Ο παρακάτω πίνακας παρέχει περιγραφές για τις ιδιότητες που χρησιμοποιείται στον ορισμό JSON. 

Ιδιότητα | Περιγραφή | Απαιτείται
-------- | ----------- | --------
Τύπος | Η ιδιότητα τύπος πρέπει να ορίσετε σε: **AzureDataLakeAnalytics**. | Ναι
όνομα λογαριασμού | Ανάλυση λίμνης Azure δεδομένων όνομα λογαριασμού. | Ναι
dataLakeAnalyticsUri | URI ανάλυση λίμνης Azure δεδομένων. |  Όχι 
εξουσιοδότηση | Εξουσιοδότηση γίνεται αυτόματα ανάκτηση κωδικού μετά κάνοντας κλικ στο κουμπί **εξουσιοδότηση** στο πρόγραμμα επεξεργασίας εργοστασίου δεδομένων και την ολοκλήρωση OAuth login.  | Ναι 
subscriptionId | Αναγνωριστικό συνδρομής Azure | Δεν (Εάν δεν καθορίζονται, τη συνδρομή από την προέλευση δεδομένων χρησιμοποιούνται). 
resourceGroupName | Όνομα ομάδας πόρων Azure |  Δεν (Εάν δεν καθορίζονται, ομάδα πόρων του εργοστασίου δεδομένων χρησιμοποιούνται).
αναγνωριστικό περιόδου λειτουργίας | αναγνωριστικό περιόδου λειτουργίας από την περίοδο λειτουργίας εξουσιοδότησης OAuth. Κάθε αναγνωριστικό περιόδου λειτουργίας είναι μοναδικό και μπορεί να χρησιμοποιηθεί μόνο μία φορά. Αναγνωριστικό δημιουργείται αυτόματα στο πρόγραμμα επεξεργασίας εργοστασίου δεδομένων την περίοδο λειτουργίας. | Ναι

Ο κωδικός εξουσιοδότησης που δημιουργήσατε, χρησιμοποιώντας το κουμπί **εξουσιοδότηση** λήξη μετά από λίγη ώρα. Ανατρέξτε στον παρακάτω πίνακα για την ώρα λήξης για διαφορετικούς τύπους λογαριασμών χρηστών. Μπορεί να δείτε το ακόλουθο σφάλμα όταν το μήνυμα ελέγχου ταυτότητας **Εάν λήξει το διακριτικό**: διαπιστευτηρίων σφάλμα λειτουργίας: invalid_grant - AADSTS70002: σφάλμα επικύρωσης διαπιστευτήρια. AADSTS70008: Έχει λήξει ή ανακληθεί την εκχώρηση πρόσβασης που παρέχεται. Αναγνωριστικό ανίχνευσης: Αναγνωριστικό συσχέτισης d18629e8-af88-43c5-88e3-d8419eb1fca1: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 χρονικής σήμανσης: 2015-12-15 21:09:31Z

 
| Τύπος χρήστη | Λήξη μετά |
| :-------- | :----------- | 
| Λογαριασμοί χρηστών δεν διαχειρίζεται Azure Active Directory (@hotmail.com, @live.com, κ.λπ.) | 12 ώρες |
| Λογαριασμοί χρηστών διαχειριζόμενο από Azure Active Directory (AAD) | Εκτελέστε 14 ημέρες μετά την τελευταία φέτα. <br/><br/>90 ημερών, εάν μια φέτα που βασίζονται σε συνδεδεμένα υπηρεσία που βασίζεται σε διακριτικό εκτελείται τουλάχιστον μία φορά κάθε 14 ημέρες. |

Για να αποφύγετε/επίλυση αυτό το σφάλμα, χρησιμοποιώντας την **εξουσιοδότηση** reauthorize κουμπί όταν το **διακριτικό λήξει** και επανάληψη ανάπτυξης συνδεδεμένων της υπηρεσίας. Μπορείτε επίσης να δημιουργήσετε τιμές για τις ιδιότητες **αναγνωριστικού περιόδου λειτουργίας** και **εξουσιοδότησης** μέσω προγραμματισμού χρησιμοποιώντας κωδικό στην επόμενη ενότητα. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>Για να δημιουργήσετε μέσω προγραμματισμού τιμές αναγνωριστικού περιόδου λειτουργίας και εξουσιοδότησης 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Ανατρέξτε στο θέμα θέματα [Κλάσης AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService τάξης](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)και [AuthorizationSessionGetResponse κλάσης](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) για λεπτομέρειες σχετικά με τις κατηγορίες δεδομένων εργοστασίου χρησιμοποιείται στον κώδικα. Προσθήκη αναφοράς σε: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll για την τάξη WindowsFormsWebAuthenticationDialog. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Δραστηριότητα U-SQL λίμνης ανάλυση δεδομένων 

Το παρακάτω τμήμα κώδικα JSON καθορίζει μια διαδικασία με μια δραστηριότητα δεδομένων λίμνης ανάλυση U-SQL. Ο ορισμός δραστηριότητας έχει μια αναφορά για την υπηρεσία συνδεδεμένες αναλυτικών στοιχείων λίμνης δεδομένων Azure που δημιουργήσατε νωρίτερα.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


Ο παρακάτω πίνακας περιγράφει τα ονόματα και τις περιγραφές ιδιοτήτων που σχετίζονται με αυτήν τη δραστηριότητα. 

Ιδιότητα | Περιγραφή | Απαιτείται
:-------- | :----------- | :--------
Τύπος | Η ιδιότητα τύπος πρέπει να οριστεί σε **DataLakeAnalyticsU-SQL**. | Ναι
scriptPath | Διαδρομή προς το φάκελο που περιέχει τη δέσμη ενεργειών U-SQL. Το όνομα του αρχείου είναι διάκριση πεζών-κεφαλαίων. | Χωρίς (Εάν χρησιμοποιείτε δέσμη ενεργειών)
scriptLinkedService | Συνδεδεμένων υπηρεσιών που συνδέεται το χώρο αποθήκευσης που περιέχει τη δέσμη ενεργειών για την προέλευση δεδομένων | Χωρίς (Εάν χρησιμοποιείτε δέσμη ενεργειών)
δέσμη ενεργειών | Καθορίστε ενσωματωμένη δέσμη ενεργειών αντί για τον καθορισμό scriptPath και scriptLinkedService. Για παράδειγμα: "δέσμη ενεργειών": "Δοκιμή της ΔΗΜΙΟΥΡΓΊΑ βάσης ΔΕΔΟΜΈΝΩΝ". | Χωρίς (Εάν χρησιμοποιείτε scriptPath και scriptLinkedService)
degreeOfParallelism | Ο μέγιστος αριθμός κόμβους ταυτόχρονη χρησιμοποιείται για την εκτέλεση της εργασίας. | Όχι
προτεραιότητα | Καθορίζει ποιες εργασίες από όλες που βρίσκονται στην ουρά πρέπει να είναι επιλεγμένο για να εκτελέσετε πρώτη. Το κάτω τον αριθμό, όσο υψηλότερη είναι η προτεραιότητα. | Όχι 
παράμετροι | Παράμετροι για τη δέσμη ενεργειών U-SQL | Όχι 

Ανατρέξτε στο θέμα [Ορισμός SearchLogProcessing.txt δέσμης ενεργειών](#script-definition) για τον ορισμό της δέσμης ενεργειών. 

## <a name="sample-input-and-output-datasets"></a>Δείγμα εισόδου και εξόδου συνόλων δεδομένων

### <a name="input-dataset"></a>Εισαγωγή συνόλου δεδομένων
Σε αυτό το παράδειγμα, τα δεδομένα εισόδου βρίσκεται σε ένα χώρο αποθήκευσης λίμνης δεδομένων Azure (SearchLog.tsv αρχείο στο φάκελο datalake/εισαγωγής). 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Σύνολο δεδομένων εξόδου
Σε αυτό το παράδειγμα, τα δεδομένα εξόδου που παράγεται από τη δέσμη ενεργειών U-SQL αποθηκεύονται σε ένα χώρο αποθήκευσης λίμνης δεδομένων Azure (datalake/εξόδου φάκελο). 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Δείγμα δεδομένων λίμνης Store συνδεδεμένες υπηρεσίας
Παρακάτω θα δείτε τον ορισμό του δείγματος χώρου αποθήκευσης λίμνης δεδομένων Azure συνδεδεμένες υπηρεσία που χρησιμοποιείται από τα σύνολα δεδομένων εισόδου/εξόδου. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

Ανατρέξτε στο θέμα [Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης λίμνης δεδομένων Azure](data-factory-azure-datalake-connector.md) άρθρο για περιγραφές ιδιοτήτων JSON. 

## <a name="sample-u-sql-script"></a>Δείγμα δέσμης ενεργειών U-SQL 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

Οι τιμές για **@in** και **@out** παραμέτρους στη δέσμη ενεργειών U-SQL μεταβιβάζονται δυναμικά από το ADF χρησιμοποιώντας την ενότητα 'παράμετροι'. Ανατρέξτε στην ενότητα 'παράμετροι' στον ορισμό διοχέτευσης.

Μπορείτε να καθορίσετε καθώς και άλλες ιδιότητες όπως degreeOfParallelism και την προτεραιότητά στον ορισμό σας διοχέτευσης για τις εργασίες που εκτελούνται σε η υπηρεσία Analytics λίμνης Azure δεδομένων.

## <a name="dynamic-parameters"></a>Δυναμικές παραμέτρους
Στον ορισμό διοχέτευσης δείγμα, και σμίκρυνση παραμέτρους εκχωρούνται με σχεδιασμένου τιμές. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

Είναι δυνατή η χρήση δυναμικών παραμέτρων αντί για αυτό. Για παράδειγμα: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

Σε αυτήν την περίπτωση, αρχεία εισόδου είναι εξακολουθεί να επιλέξει από το φάκελο /datalake/input και αρχεία εξόδου δημιουργούνται στο φάκελο /datalake/output. Τα ονόματα αρχείων είναι δυναμικά με βάση την ώρα έναρξης φέτα.  