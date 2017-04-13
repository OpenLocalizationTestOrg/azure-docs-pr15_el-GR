<properties 
    pageTitle="Δεδομένα Factory - αρχείο καταγραφής αλλαγών API .NET | Microsoft Azure" 
    description="Περιγράφει τις πρόσφατες αλλαγές, η δυνατότητα προσθήκες, διορθώσεις σφαλμάτων κ.λπ... σε μια συγκεκριμένη έκδοση του .NET API για την προέλευση δεδομένων Azure." 
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
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure Factory δεδομένων - αρχείο καταγραφής αλλαγών .NET API 
Σε αυτό το άρθρο παρέχει πληροφορίες σχετικά με τις αλλαγές στους Azure SDK εργοστασίου δεδομένων σε μια συγκεκριμένη έκδοση. Μπορείτε να βρείτε την πιο πρόσφατη πακέτου NuGet για εργοστασίου δεδομένων Azure [εδώ](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>Έκδοση 4.11.0
Η δυνατότητα προσθήκες:

- Έχουν προστεθεί τους ακόλουθους τύπους συνδεδεμένων υπηρεσιών:
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- Έχουν προστεθεί οι παρακάτω τύποι συνόλου δεδομένων: 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- Έχουν προστεθεί οι παρακάτω τύποι προέλευσης αντίγραφο:
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>Έκδοση 4.10.0
- Έχουν προστεθεί οι ακόλουθες προαιρετικές ιδιότητες TextFormat:
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- Έχουν προστεθεί τους ακόλουθους τύπους συνδεδεμένων υπηρεσιών:
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- Έχουν προστεθεί οι παρακάτω τύποι συνόλου δεδομένων:
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- Έχουν προστεθεί οι παρακάτω τύποι προέλευσης αντίγραφο:
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- Προσθήκη ιδιοτήτων [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) AzureMLBatchExecutionActivity 
    - Ενεργοποίηση διέρχεται πολλών web υπηρεσίας δεδομένα εισόδου σε μια έρευνα Azure μηχανικής εκμάθησης


## <a name="version-491"></a>Έκδοση 4.9.1

### <a name="bug-fix"></a>Διόρθωση σφάλματος

- Υποβάθμιση ελέγχου ταυτότητας που βασίζεται σε WebApi για [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Έκδοση 4.9.0

### <a name="feature-additions"></a>Η δυνατότητα προσθήκες

- Προσθέστε ιδιότητες [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) και [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) CopyActivity. Για λεπτομέρειες σχετικά με τη δυνατότητα, ανατρέξτε στο θέμα [Αντιγραφή σταδιακή](data-factory-copy-activity-performance.md#staged-copy) .


### <a name="bug-fix"></a>Διόρθωση σφάλματος

- Παρουσιάστε ένα υπερφόρτωσης του μέθοδο [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) , η οποία λαμβάνει μια παρουσία [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) .
- Σήμανση [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) και [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) ως προαιρετικό στο CopySink.

## <a name="version-480"></a>Έκδοση 4.8.0

### <a name="feature-additions"></a>Η δυνατότητα προσθήκες
- Αντιγραφή τύπου δραστηριότητας για την ενεργοποίηση της ρύθμισης επιδόσεων αντίγραφο έχουν προστεθεί οι ακόλουθες προαιρετικές ιδιότητες:
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Έκδοση 4.7.0

### <a name="feature-additions"></a>Η δυνατότητα προσθήκες
* Προσθήκη νέου τύπου StorageFormat [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) τύπο για να αντιγράψετε τα αρχεία σε μορφή στήλης (ORC) βελτιστοποιημένη γραμμή.
* Προσθέστε ιδιότητες [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) και PolyBaseSettings SqlDWSink.
    * Επιτρέπει τη χρήση του PolyBase για να αντιγράψετε δεδομένα σε αποθήκη δεδομένων του SQL.

## <a name="version-461"></a>Έκδοση 4.6.1

### <a name="bug-fixes"></a>Διορθώσεις σφαλμάτων
* Διορθώνει αίτηση HTTP για καταχώρηση δραστηριότητα των windows.
    * Καταργεί το όνομα της ομάδας πόρων και το όνομα εργοστασίου δεδομένων από το φορτίο αίτηση.

## <a name="version-460"></a>Έκδοση 4.6.0

### <a name="feature-additions"></a>Η δυνατότητα προσθήκες

- Έχουν προστεθεί [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx)τις ακόλουθες ιδιότητες:
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [Σύνολα δεδομένων](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- Έχουν προστεθεί [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx)τις ακόλουθες ιδιότητες:
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- Προστέθηκε νέα [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) πληκτρολογήστε [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) τύπου για τον ορισμό των οποίων τα δεδομένα είναι σε μορφή JSON συνόλων δεδομένων. 

## <a name="version-450"></a>Έκδοση 4.5.0

### <a name="feature-additions"></a>Η δυνατότητα προσθήκες
* Πρόσθετες [Λειτουργίες λίστα παραθύρου δραστηριότητα](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
    * Προστέθηκε μεθόδους για την ανάκτηση windows δραστηριότητας με τα φίλτρα που βασίζονται στους τύπους οντότητα (δηλαδή, εργοστάσια δεδομένων, σύνολα δεδομένων, αγωγούς και δραστηριότητες).
* Έχουν προστεθεί τους ακόλουθους τύπους συνδεδεμένων υπηρεσιών: 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Έχουν προστεθεί οι παρακάτω τύποι συνόλου δεδομένων: 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Έχουν προστεθεί οι παρακάτω τύποι προέλευσης αντίγραφο:  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Έκδοση 4.4.0

### <a name="feature-additions"></a>Η δυνατότητα προσθήκες

- Ο παρακάτω τύπος συνδεδεμένων υπηρεσίας έχει προστεθεί ως προελεύσεις δεδομένων και δέκτες για αντιγραφή δραστηριότητες:
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Για γενικές πληροφορίες και παραδείγματα, ανατρέξτε στο θέμα [Υπηρεσία συνδεδεμένων συσχετισμών Ασφαλείας αποθήκευσης Azure](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) . 

## <a name="version-430"></a>Έκδοση 4.3.0

### <a name="feature-additions"></a>Η δυνατότητα προσθήκες

- Το παρακάτω υπάρχει τύποι συνδεδεμένων υπηρεσιών έχουν προστεθεί ως προελεύσεις δεδομένων για το αντίγραφο δραστηριότητες:
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Για γενικές πληροφορίες και παραδείγματα, ανατρέξτε στο θέμα [Μετακίνηση δεδομένων από HDFS χρησιμοποιώντας εργοστασίου δεδομένων](data-factory-hdfs-connector.md) . 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Για γενικές πληροφορίες και παραδείγματα, ανατρέξτε στο θέμα [Μετακίνηση δεδομένων ODBC από δεδομένα αποθηκεύει χρησιμοποιώντας Azure εργοστασίου δεδομένων](data-factory-odbc-connector.md) . 

## <a name="version-420"></a>Έκδοση 4.2.0

### <a name="feature-additions"></a>Η δυνατότητα προσθήκες

- Το παρακάτω νέου τύπου δραστηριότητας έχει προστεθεί: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Για λεπτομέρειες σχετικά με τη δραστηριότητα, ανατρέξτε στο θέμα [Ενημέρωση ML Azure μοντέλα χρησιμοποιώντας τη δραστηριότητα πόρων ενημερωμένη έκδοση](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- Ένα νέο προαιρετικό ιδιότητα [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) έχει προστεθεί στην [AzureMLLinkedService τάξης](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx). 
- Ιδιότητες [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) και [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) έχουν προστεθεί στην κλάση [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) . 
- Ρύθμιση παραμέτρων του τα χρονικά όρια για κλήσεις προγράμματος-πελάτη στην υπηρεσία εργοστασίου δεδομένων. 


## <a name="version-410"></a>Έκδοση 4.1.0

### <a name="feature-additions"></a>Η δυνατότητα προσθήκες
* Έχουν προστεθεί τους ακόλουθους τύπους συνδεδεμένων υπηρεσιών: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Έχουν προστεθεί οι παρακάτω τύποι δραστηριότητας: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Έχουν προστεθεί οι παρακάτω τύποι συνόλου δεδομένων: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Έχουν προστεθεί τους εξής τύπους προέλευσης και δέκτη για αντιγραφή δραστηριότητας:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>Έκδοση 4.0.1

### <a name="breaking-changes"></a>Διακοπή αλλαγές
Οι παρακάτω κλάσεις έχουν μετονομαστεί. Τα νέα ονόματα έχουν τα αρχικά ονόματα κλάσεων πριν από την έκδοση 4.0.0. 
 
Δώστε ένα όνομα στο 4.0.0 | Δώστε ένα όνομα στο 4.0.1
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>Έκδοση 4.0.0

### <a name="breaking-changes"></a>Διακοπή αλλαγές



- Οι παρακάτω κλάσεις/διασυνδέσεις έχουν μετονομαστεί.

| Παλιό όνομα | Νέο όνομα |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Πίνακας | [Σύνολο δεδομένων](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- Οι μέθοδοι **λίστα** επιστρέφουν αποτελέσματα σελιδοποιημένος τώρα. Εάν η απόκριση περιέχει μια ιδιότητα **NextLink** μη κενή, την εφαρμογή-πελάτη πρέπει να συνεχιστεί η εξαγωγή στην επόμενη σελίδα, μέχρι να επιστρέφονται όλες τις σελίδες.  Ακολουθεί ένα παράδειγμα: 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **Λίστα** διοχέτευσης API επιστρέφει μόνο η σύνοψη δίκτυο αγωγών αντί για πλήρεις λεπτομέρειες. Για παράδειγμα, οι δραστηριότητες σε μια σύνοψη διοχέτευσης περιέχει μόνο όνομα και τύπος.

### <a name="feature-additions"></a>Η δυνατότητα προσθήκες
- Η κλάση [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) υποστηρίζει δύο νέες ιδιότητες, **SliceIdentifierColumnName** και **SqlWriterCleanupScript**, για την υποστήριξη idempotent Αντιγραφή σε αποθήκη δεδομένων του SQL Azure. Ανατρέξτε στο άρθρο [Αποθήκη δεδομένων του SQL Azure](data-factory-azure-sql-data-warehouse-connector.md) , συγκεκριμένα, το [μηχανισμό 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) και [2 μηχανισμό](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) ενότητες, για λεπτομέρειες σχετικά με αυτές τις ιδιότητες.

- Υποστηρίζουμε τώρα εκτελείται αποθηκευμένη διαδικασία σε σχέση με βάση δεδομένων SQL Azure και αποθήκη δεδομένων του SQL Azure προελεύσεων ως τμήμα της δραστηριότητας αντίγραφο. Οι κλάσεις [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) και [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) έχουν τις ακόλουθες ιδιότητες: **SqlReaderStoredProcedureName** και **StoredProcedureParameters**. Ανατρέξτε στα άρθρα [Βάση δεδομένων SQL Azure](data-factory-azure-sql-connector.md#sqlsource) και [Αποθήκη δεδομένων του SQL Azure](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) σε Azure.com για λεπτομέρειες σχετικά με αυτές τις ιδιότητες.  