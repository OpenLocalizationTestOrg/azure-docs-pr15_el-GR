<properties
    pageTitle="Δημιουργία δυνατότητες για δεδομένα χώρο αποθήκευσης αντικειμένων blob του Azure χρησιμοποιώντας Panda | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε δυνατότητες για τα δεδομένα που είναι αποθηκευμένα στο κοντέινερ αντικειμένων blob του Azure με το πακέτο Panda Python."
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
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Δημιουργία δυνατότητες για δεδομένα χώρο αποθήκευσης αντικειμένων blob του Azure χρησιμοποιώντας Panda

Αυτό το έγγραφο περιγράφει τον τρόπο δημιουργίας δυνατοτήτων για τα δεδομένα που είναι αποθηκευμένα στο κοντέινερ αντικειμένων blob του Azure χρησιμοποιώντας το πακέτο [Pandas](http://pandas.pydata.org/) Python. Μετά τη διάρθρωση πώς να φορτώσετε τα δεδομένα σε ένα πλαίσιο Panda δεδομένων, που εμφανίζει τον τρόπο δημιουργίας έλαβε δυνατότητες δέσμες ενεργειών Python με ένδειξη τιμές και binning δυνατότητες.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Το **μενού** συνδέσεις σε θέματα τα οποία περιγράφουν τον τρόπο δημιουργίας δυνατοτήτων για τα δεδομένα σε διάφορα περιβάλλοντα. Αυτή η εργασία είναι ένα βήμα για τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Σε αυτό το άρθρο προϋποθέτει ότι έχετε δημιουργήσει ένα λογαριασμό χώρο αποθήκευσης αντικειμένων blob του Azure και έχετε αποθηκεύσει τα δεδομένα σας εκεί. Εάν χρειάζεστε οδηγίες για να ρυθμίσετε ένα λογαριασμό, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού αποθήκευσης Azure](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>Φόρτωση των δεδομένων σε ένα πλαίσιο δεδομένων Pandas
Για να εξερευνήσετε και να διαχειριστείτε ένα σύνολο δεδομένων, αυτό πρέπει να γίνει λήψη από την προέλευση blob σε ένα τοπικό αρχείο το οποίο, στη συνέχεια, είναι δυνατή η φόρτωση σε ένα πλαίσιο δεδομένων Pandas. Ακολουθούν τα βήματα για να παρακολουθήσετε για αυτήν τη διαδικασία:

1. Λήψη των δεδομένων από το Azure αντικειμένων blob με τον ακόλουθο κώδικα Python δείγμα με υπηρεσία blob. Αντικαταστήστε τη μεταβλητή στον κώδικα κάτω από το στοιχείο με τις συγκεκριμένες τιμές:

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


2. Διαβάστε τα δεδομένα σε ένα πλαίσιο Pandas δεδομένων-από το αρχείο που έχετε λάβει.

        #LOCALFILE is the file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Τώρα είστε έτοιμοι να εξερευνήσετε τα δεδομένα και να δημιουργήσετε δυνατότητες σε αυτό το σύνολο δεδομένων.

##<a name="blob-featuregen"></a>Δυνατότητα δημιουργίας

Στις επόμενες δύο ενότητες δείχνουν τον τρόπο για τη δημιουργία κατηγοριών δυνατότητες με ένδειξη τιμές και binning δυνατότητες χρήση Python δεσμών ενεργειών.

###<a name="blob-countfeature"></a>Ένδειξη τιμή με βάση τη δυνατότητα γενιάς

Έλαβε δυνατότητες μπορούν να δημιουργηθούν ως εξής:

1. Έλεγχος την κατανομή των κατηγοριών στήλη:

        dataframe_blobdata['<categorical_column>'].value_counts()

2. Δημιουργία δείκτης τιμών για κάθε μία από τις τιμές της στήλης

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Συμμετοχή σε στήλη δεικτών με το αρχικό πλαίσιο δεδομένων

            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Καταργήστε την ίδια τη μεταβλητή αρχικά:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

###<a name="blob-binningfeature"></a>Binning δυνατότητα γενιάς

Για τη δημιουργία binned δυνατότητες, συνεχίσουμε ως εξής:

1. Προσθέστε μια ακολουθία των στηλών σε μια στήλη αριθμητικών κλάσης

        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)

2. Μετατροπή binning σε μια ακολουθία δυαδική μεταβλητών

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')

3. Τέλος, συμμετοχή τις μεταβλητές εικονική προς το αρχικό πλαίσιο δεδομένων

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

##<a name="sql-featuregen"></a>Εγγραφή δεδομένων αντικειμένων blob του Azure και κατανάλωση στο Azure μηχανικής εκμάθησης

Αφού εξετάσετε τα δεδομένα και να δημιουργήσει τις δυνατότητες που είναι απαραίτητο, μπορείτε να αποστείλετε τα δεδομένα (δειγματοληψία ή featurized) σε μια Azure blob και την κατανάλωση στο Azure μηχανικής εκμάθησης ακολουθώντας τα παρακάτω βήματα: Σημειώστε ότι οι πρόσθετες δυνατότητες μπορούν να δημιουργηθούν σε το Azure μηχανικής εκμάθησης Studio καθώς και.
1. Εγγραφή στο πλαίσιο δεδομένα στο τοπικό αρχείο

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Αποστολή των δεδομένων στο αντικειμένων blob του Azure ως εξής:

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
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Τώρα μπορείτε να διαβάσετε τα δεδομένα από το αντικείμενο blob χρησιμοποιώντας τη λειτουργική μονάδα Azure μηχανικής εκμάθησης [Εισαγωγή δεδομένων](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) , όπως φαίνεται στην παρακάτω οθόνη:

![πρόγραμμα ανάγνωσης blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)
