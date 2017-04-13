<properties 
    pageTitle="Δείγμα δεδομένων σε Azure χώρο αποθήκευσης αντικειμένων blob | Microsoft Azure" 
    description="Δείγμα δεδομένων στο χώρο αποθήκευσης Blob του Azure" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Δείγμα δεδομένων σε Azure χώρο αποθήκευσης αντικειμένων blob


Αυτό το έγγραφο καλύπτει δειγματοληψία δεδομένα που είναι αποθηκευμένα στο χώρο αποθήκευσης αντικειμένων blob του Azure κατά τη λήψη μέσω προγραμματισμού και, στη συνέχεια, δειγματοληψία χρησιμοποιώντας διαδικασίες γραμμένο σε Python.

**Δείγμα γιατί τα δεδομένα σας;**
Εάν το σύνολο δεδομένων που σχεδιάζετε να αναλύσετε είναι μεγάλο, είναι συνήθως καλή ιδέα να τα δεδομένα για να μειώσετε το μέγεθος μικρότερο αλλά αντιπρόσωπος και πιο εύχρηστη δειγμάτων προς τα κάτω. Αυτό διευκολύνει την κατανόηση των δεδομένων, Εξερεύνηση και η δυνατότητα μηχανικής. Είναι ο ρόλος του για τη διαδικασία ανάλυσης Cortana για να ενεργοποιήσετε την γρήγορα τη δημιουργία πρωτοτύπων του λειτουργίες επεξεργασίας δεδομένων και μηχανικής εκμάθησης μοντέλων.

Το **μενού** παρακάτω συνδέσεις σε θέματα τα οποία περιγράφουν τον τρόπο δείγμα δεδομένων από διάφορα περιβάλλοντα χώρο αποθήκευσης. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Αυτή η εργασία δειγματοληψία είναι ένα βήμα για τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Λήψη και κάτω-δείγμα δεδομένων
1. Λήψη δεδομένων από το χώρο αποθήκευσης αντικειμένων blob του Azure χρησιμοποιώντας την υπηρεσία blob από τον παρακάτω κώδικα Python δείγμα: 

        from azure.storage.blob import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))

2. Ανάγνωση δεδομένων μέσα σε ένα πλαίσιο Pandas δεδομένων-από το αρχείο που έχουν ληφθεί παραπάνω.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Δείγματα προς τα κάτω τα δεδομένα χρησιμοποιώντας το `numpy`του `random.choice` ως εξής:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Τώρα μπορείτε να εργαστείτε με το παραπάνω πλαίσιο δεδομένων με το δείγμα 1 τοις εκατό για περαιτέρω Εξερεύνηση και η δυνατότητα γενιάς.

##<a name="heading"></a>Αποστολή δεδομένων και να το διαβάσουν σε Azure μηχανικής εκμάθησης

Μπορείτε να χρησιμοποιήσετε το παρακάτω δείγμα κώδικα για να δειγμάτων προς τα κάτω τα δεδομένα και να χρησιμοποιήσετε απευθείας στο Azure ML:

1. Γράψτε το πλαίσιο δεδομένων σε ένα τοπικό αρχείο

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Αποστείλετε το τοπικό αρχείο μια αντικειμένων blob του Azure χρησιμοποιώντας το παρακάτω δείγμα κώδικα:

        from azure.storage.blob import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. Ανάγνωση των δεδομένων από το αντικειμένων blob του Azure χρησιμοποιώντας Azure ML [Εισαγωγή δεδομένων](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) , όπως φαίνεται στην παρακάτω εικόνα:
 
![πρόγραμμα ανάγνωσης blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 
