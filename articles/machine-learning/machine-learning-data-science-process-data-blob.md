<properties 
    pageTitle="Επεξεργάζεται δεδομένα Azure blob με analytics για προχωρημένους | Microsoft Azure" 
    description="Διαδικασία δεδομένα στο χώρο αποθήκευσης αντικειμένων Blob Azure." 
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

#<a name="heading"></a>Επεξεργαστείτε τα δεδομένα Azure blob με analytics για προχωρημένους

Αυτό το έγγραφο καλύπτει την Εξερεύνηση δεδομένων και δυνατότητες παραγωγής από δεδομένα που είναι αποθηκευμένα στο χώρο αποθήκευσης αντικειμένων Blob Azure. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Η φόρτωση των δεδομένων μέσα σε ένα πλαίσιο δεδομένων Pandas
Για να εξερευνήσετε και να χειριστείτε ένα dataset, αυτό πρέπει να γίνει λήψη από το αρχείο προέλευσης blob σε ένα τοπικό αρχείο, το οποίο στη συνέχεια μπορεί να φορτωθεί σε ένα πλαίσιο δεδομένων Pandas. Ακολουθούν τα βήματα για να ακολουθήσετε αυτήν τη διαδικασία:

1. Λήψη δεδομένων από Azure blob με το ακόλουθο δείγμα κώδικα Python που χρησιμοποιεί την υπηρεσία blob. Αντικαταστήστε τη μεταβλητή στον κώδικα παρακάτω με τις τιμές σας: 

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


2. Η ανάγνωση των δεδομένων μέσα σε ένα πλαίσιο Pandas δεδομένων-από το αρχείο που λάβατε.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Τώρα είστε έτοιμοι για να εξερευνήσετε τα δεδομένα και να δημιουργήσει τις δυνατότητες σε αυτό το dataset.


##<a name="blob-dataexploration"></a>Εξερεύνηση δεδομένων

Ακολουθούν μερικά παραδείγματα τρόπους για να εξερευνήσετε δεδομένα χρησιμοποιώντας Pandas:

1. Εξετάστε τον αριθμό γραμμών και στηλών 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Εξετάστε την πρώτη ή την τελευταία μερικές γραμμές στο dataset ως εξής:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Έλεγχος του τύπου δεδομένων κάθε στήλης που έχει εισαχθεί ως χρησιμοποιώντας το ακόλουθο δείγμα κώδικα
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Ελέγξτε τα βασικά στατιστικά στοιχεία για τις στήλες του συνόλου δεδομένων ως εξής
 
        dataframe_blobdata.describe()
    
5. Δείτε τον αριθμό των εγγραφών για κάθε τιμή της στήλης ως εξής

        dataframe_blobdata['<column_name>'].value_counts()

6. Πλήθος τιμών λείπει σε σχέση με τον πραγματικό αριθμό των εγγραφών σε κάθε στήλη χρησιμοποιώντας το ακόλουθο δείγμα κώδικα

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Εάν έχετε τιμές που λείπουν για μια συγκεκριμένη στήλη στα δεδομένα, μπορείτε να απορρίψετε τους ως εξής:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Ένας άλλος τρόπος για να αντικαταστήσετε τις τιμές που λείπουν είναι με τη συνάρτηση mode:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Δημιουργήστε ένα παρατηρητήριο ιστόγραμμα χρησιμοποιώντας μεταβαλλόμενο αριθμό θέσεων αποθήκης για να απεικονίσετε τη διανομή μιας μεταβλητής 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Κοιτάξτε τους συσχετισμούς μεταξύ των μεταβλητών, χρησιμοποιώντας ένα scatterplot ή χρησιμοποιώντας τη συνάρτηση συσχετισμού ενσωματωμένη

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="blob-featuregen"></a>Δυνατότητα δημιουργίας
    
Μπορέσουμε να δημιουργήσουμε δυνατότητες χρησιμοποιώντας Python ως εξής:

###<a name="blob-countfeature"></a>Τιμή δείκτη με βάση τη δυνατότητα δημιουργίας

Κατηγορική δυνατότητες μπορούν να δημιουργηθούν ως εξής:

1. Εξετάστε την κατανομή της κατηγορική στήλης:
    
        dataframe_blobdata['<categorical_column>'].value_counts()

2. Δημιουργία δεικτών τιμών για κάθε μία από τις τιμές των στηλών

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Συμμετοχή στη στήλη δεικτών με το αρχικό πλαίσιο δεδομένων 
 
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Καταργήστε την ίδια τη αρχική μεταβλητή:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    
###<a name="blob-binningfeature"></a>Binning δυνατότητα δημιουργίας

Για τη δημιουργία binned δυνατότητες, συνεχίσουμε ως εξής:

1. Προσθήκη μιας σειράς στηλών σε μια αριθμητική στήλη η θέση αποθήκης
 
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
        
2. Μετατροπή binning σε μια ακολουθία δυαδικές μεταβλητές

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    
3. Τέλος, συμμετοχή των μεταβλητών του ανδρεικέλου προς τα πίσω στο αρχικό πλαίσιο δεδομένων

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool) 


##<a name="sql-featuregen"></a>Εγγραφή δεδομένων Azure blob και καταναλώνουν στην εκμάθηση μηχανής Azure

Αφού εξετάσετε τα δεδομένα και να δημιουργήσει τις απαραίτητες δυνατότητες, μπορείτε να αποστείλετε τα δεδομένα (δειγματοληψίας ή featurized) σε μια Azure blob και αναλώσετε στην εκμάθηση μηχανής Azure ακολουθώντας τα παρακάτω βήματα: Σημειώστε ότι πρόσθετες δυνατότητες μπορούν να δημιουργηθούν με το Azure υπολογιστή εκμάθηση Studio καθώς και. 
1. Εγγραφή το πλαίσιο δεδομένων στο τοπικό αρχείο

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Αποστολή των δεδομένων Azure blob ως εξής:

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

3. Τώρα τα δεδομένα μπορεί να διαβαστεί από το αντικείμενο blob χρησιμοποιώντας την εκμάθηση μηχανής Azure [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα, όπως φαίνεται στην παρακάτω οθόνη:
 
![πρόγραμμα ανάγνωσης blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
