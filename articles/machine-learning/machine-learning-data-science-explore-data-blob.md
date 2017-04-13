<properties 
    pageTitle="Εξερεύνηση δεδομένων στο χώρο αποθήκευσης αντικειμένων blob του Azure με Pandas | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να εξερευνήσετε τα δεδομένα που είναι αποθηκευμένα στο κοντέινερ αντικειμένων blob του Azure χρησιμοποιώντας Pandas." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Εξερεύνηση δεδομένων στο χώρο αποθήκευσης αντικειμένων blob του Azure με Pandas

Αυτό το έγγραφο περιγράφει πώς μπορείτε να εξερευνήσετε τα δεδομένα που είναι αποθηκευμένα στο κοντέινερ αντικειμένων blob του Azure χρησιμοποιώντας [Pandas](http://pandas.pydata.org/) Python πακέτο.

Το παρακάτω **μενού** συνδέσεις σε θέματα τα οποία περιγράφουν πώς μπορείτε να χρησιμοποιήσετε εργαλεία για την Εξερεύνηση των δεδομένων από διάφορα περιβάλλοντα χώρο αποθήκευσης. Αυτή η εργασία είναι ένα βήμα της [Διεργασίας Science δεδομένων]().

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Σε αυτό το άρθρο προϋποθέτει ότι έχετε:

* Δημιουργία λογαριασμού Azure χώρου αποθήκευσης. Εάν χρειάζεστε οδηγίες, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού αποθήκευσης Azure](../storage/storage-create-storage-account.md#create-a-storage-account)
* Αποθηκεύονται τα δεδομένα σας σε ένα λογαριασμό του χώρου αποθήκευσης αντικειμένων blob του Azure. Εάν χρειάζεστε οδηγίες, ανατρέξτε στο θέμα [Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης Azure](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Φόρτωση των δεδομένων σε μια DataFrame Pandas
Για να εξερευνήσετε και να χειρίζονται ένα σύνολο δεδομένων, αυτό πρέπει να πρώτα να ληφθούν από την προέλευση blob σε ένα τοπικό αρχείο, το οποίο, στη συνέχεια, είναι δυνατή η φόρτωση σε ένα DataFrame Pandas. Ακολουθούν τα βήματα για να παρακολουθήσετε για αυτήν τη διαδικασία:

1. Λήψη των δεδομένων από το Azure αντικειμένων blob με το παρακάτω δείγμα κώδικα Python με υπηρεσία blob. Αντικαταστήστε τη μεταβλητή τον παρακάτω κώδικα με τις συγκεκριμένες τιμές: 

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

##<a name="blob-dataexploration"></a>Παραδείγματα Εξερεύνηση δεδομένων με χρήση Pandas

Ακολουθούν μερικά παραδείγματα τρόπων Εξερεύνηση δεδομένων με χρήση Pandas:

1. Έλεγχος τον **αριθμό των γραμμών και στηλών** 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **Έλεγχος** το πρώτο ή το τελευταίο μερικές **γραμμές** σε του συνόλου δεδομένων παρακάτω:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Έλεγχος του **τύπου δεδομένων** κάθε στήλη έχει εισαχθεί ως χρησιμοποιώντας το παρακάτω δείγμα κώδικα
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Ελέγξτε τη **Βασική στατιστικά στοιχεία** για τις στήλες του συνόλου δεδομένων ως εξής
 
        dataframe_blobdata.describe()
    
5. Δείτε τον αριθμό των καταχωρήσεων για κάθε τιμή στη στήλη ως εξής

        dataframe_blobdata['<column_name>'].value_counts()

6. **Τιμές που λείπουν καταμέτρηση** έναντι ο πραγματικός αριθμός καταχωρήσεων σε κάθε στήλη χρησιμοποιώντας το παρακάτω δείγμα κώδικα

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Εάν έχετε **τιμές που λείπουν** για μια συγκεκριμένη στήλη των δεδομένων, μπορείτε να απορρίψετε τους ως εξής:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Ένας άλλος τρόπος για να αντικαταστήσετε τιμές που λείπουν είναι με τη συνάρτηση mode:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Δημιουργήστε ένα **ιστόγραμμα** σχεδίασης με χρήση μεταβλητών αριθμό των θέσεων αποθήκευσης για να απεικονίσετε την κατανομή μιας μεταβλητής 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Εξετάστε **τους συσχετισμούς** μεταξύ μεταβλητές χρησιμοποιώντας μια scatterplot ή με χρήση της συνάρτησης ενσωματωμένες συσχέτισης

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
