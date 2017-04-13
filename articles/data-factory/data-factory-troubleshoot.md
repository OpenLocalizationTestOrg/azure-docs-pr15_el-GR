<properties 
    pageTitle="Αντιμετώπιση προβλημάτων του Azure εργοστασίου δεδομένων" 
    description="Μάθετε τον τρόπο αντιμετώπισης προβλημάτων με τη χρήση Azure εργοστασίου δεδομένων." 
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
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Αντιμετώπιση προβλημάτων εργοστασίου δεδομένων
Σε αυτό το άρθρο παρέχει συμβουλές αντιμετώπισης προβλημάτων για ζητήματα κατά τη χρήση Azure εργοστασίου δεδομένων. Σε αυτό το άρθρο δεν συμπεριλαμβάνεται σε λίστα όλα τα πιθανά προβλήματα όταν χρησιμοποιείτε την υπηρεσία, αλλά να καλύπτει ορισμένα ζητήματα και τις γενικές συμβουλές αντιμετώπισης προβλημάτων.   

## <a name="troubleshooting-tips"></a>Συμβουλές αντιμετώπισης προβλημάτων

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Σφάλμα: Η συνδρομή δεν έχει καταχωρηθεί για να χρησιμοποιήσετε το χώρο ονομάτων 'Microsoft.DataFactory'
Εάν λάβετε αυτό το σφάλμα, η υπηρεσία παροχής πόρων εργοστασίου δεδομένων Azure δεν έχει καταχωρηθεί στον υπολογιστή σας. Κάντε τα εξής: 

1. Εκκινήστε το Azure PowerShell. 
2. Συνδεθείτε στο λογαριασμό σας Azure χρησιμοποιώντας την ακόλουθη εντολή.
        AzureRmAccount σύνδεσης 
3. Εκτελέστε την ακόλουθη εντολή για την καταχώρηση της υπηρεσίας παροχής Azure εργοστασίου δεδομένων.
        REGISTER-AzureRmResourceProvider - ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Πρόβλημα: Μη εξουσιοδοτημένη σφάλμα κατά την εκτέλεση ενός cmdlet εργοστασίου δεδομένων
Μάλλον δεν χρησιμοποιείτε το λογαριασμός Azure προς τα δεξιά ή τη συνδρομή με το PowerShell Azure. Χρησιμοποιήστε τα ακόλουθα cmdlet για να επιλέξετε το λογαριασμός Azure προς τα δεξιά και να χρησιμοποιήσετε με το PowerShell Azure συνδρομή. 

1. Σύνδεση-AzureRmAccount - Χρησιμοποιήστε το Αναγνωριστικό δεξιά χρήστη και τον κωδικό πρόσβασης
2. Get-AzureRmSubscription - δείτε όλες τις συνδρομές για το λογαριασμό. 
3. Επιλέξτε AzureRmSubscription &lt;στο όνομα της συνδρομής&gt; -επιλέξτε τη συνδρομή από δεξιά. Χρησιμοποιήστε το ίδιο με αυτό που χρησιμοποιείτε για να δημιουργήσετε μια προέλευση δεδομένων στην πύλη του Azure.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Πρόβλημα: Αποτύχει να εκκινήσετε εγκατάσταση Express πύλης διαχείρισης δεδομένων από την πύλη του Azure
Το πρόγραμμα εγκατάστασης Express για την πύλη διαχείρισης δεδομένων απαιτεί Internet Explorer ή ένα συμβατό πρόγραμμα περιήγησης Microsoft ένα κλικ. Εάν το πρόγραμμα εγκατάστασης Express δεν μπορεί να ξεκινήσει, κάντε ένα από τα εξής: 

- Χρήση του Internet Explorer ή ένα συμβατό πρόγραμμα περιήγησης Microsoft ένα κλικ.

    Εάν χρησιμοποιείτε το Chrome, μεταβείτε στο [χώρο αποθήκευσης web Chrome](https://chrome.google.com/webstore/), με την αναζήτηση λέξεων-κλειδιών "Ένα κλικ", επιλέξτε μία από τις επεκτάσεις ένα κλικ και να το εγκαταστήσετε. 
    
    Κάντε το ίδιο για το Firefox (εγκατάσταση του προσθέτου). Κάντε κλικ στο κουμπί Άνοιγμα μενού στη γραμμή εργαλείων (τρεις οριζόντιες γραμμές στην επάνω δεξιά γωνία), κάντε κλικ στην επιλογή πρόσθετα, με λέξεις-κλειδιά "Ένα κλικ" για την αναζήτηση, επιλέξτε μία από τις επεκτάσεις ένα κλικ και να το εγκαταστήσετε. 

- Χρησιμοποιήστε τη **Μη αυτόματη εγκατάσταση** σύνδεση εμφανίζεται στην το ίδιο blade στην πύλη. Μπορείτε να χρησιμοποιήσετε αυτήν την προσέγγιση για να κάνετε λήψη του αρχείου εγκατάστασης και εκτελέστε το με μη αυτόματο τρόπο. Μετά την εγκατάσταση είναι επιτυχής, μπορείτε να δείτε το παράθυρο διαλόγου Ρύθμιση παραμέτρων της πύλης διαχείρισης δεδομένων. Αντιγραφή του **αριθμού-κλειδιού** από την οθόνη πύλης και χρησιμοποιήστε το στη Διαχείριση παραμέτρων για να καταχωρήσετε με μη αυτόματο τρόπο την πύλη με την υπηρεσία.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Πρόβλημα: Αποτύχει για να συνδεθείτε με τον SQL Server εσωτερικής εγκατάστασης 
Εκκίνηση **Διαχείριση παραμέτρων πύλης διαχείρισης δεδομένων** στον υπολογιστή πύλης και χρησιμοποιήστε την καρτέλα **Αντιμετώπιση προβλημάτων** για να ελέγξετε τη σύνδεση με τον SQL Server από τον υπολογιστή πύλης. Ανατρέξτε στο θέμα [πύλης αντιμετώπιση προβλημάτων](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) για συμβουλές σχετικά με την αντιμετώπιση προβλημάτων σύνδεσης/πύλης θέματα που σχετίζονται με.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Πρόβλημα: Φέτες εισόδου είναι σε αναμονή για ποτέ κατάσταση

Τις φέτες μπορεί να είναι σε **Αναμονή** κατάσταση λόγω διάφορους λόγους. Μία από τις κοινές αιτίες είναι ότι η ιδιότητα **εξωτερικών** δεν έχει οριστεί στην **τιμή true**. Κάθε σύνολο δεδομένων που παράγεται εκτός του αντικειμένου του Azure εργοστασίου δεδομένων πρέπει να επισημαίνεται με **εξωτερική** ιδιότητα. Αυτή η ιδιότητα δηλώνει ότι τα δεδομένα είναι εξωτερικών και δεν υποστηρίζεται από οποιαδήποτε αγωγούς μέσα την προέλευση δεδομένων. Τις φέτες δεδομένων επισημαίνονται ως **έτοιμοι** όταν τα δεδομένα είναι διαθέσιμο στο χώρο αποθήκευσης αντίστοιχα. 

Δείτε το παρακάτω παράδειγμα για τη χρήση της ιδιότητας **εξωτερικών** . Προαιρετικά, μπορείτε να καθορίσετε **externalData*** όταν ορίζετε εξωτερικών στην τιμή true.

Δείτε το άρθρο [σύνολα δεδομένων](data-factory-create-datasets.md) για περισσότερες λεπτομέρειες σχετικά με αυτήν την ιδιότητα.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

Για να επιλύσετε το σφάλμα, προσθέστε την ιδιότητα **εξωτερικών** και την ενότητα προαιρετικό **externalData** στον ορισμό JSON του εισαγωγής πίνακα και δημιουργήστε ξανά τον πίνακα. 

### <a name="problem-hybrid-copy-operation-fails"></a>Πρόβλημα: Υβριδική λειτουργία Αντιγραφή θα αποτύχει
Ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων της πύλης](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) για τα βήματα για την αντιμετώπιση προβλημάτων με την αντιγραφή προς/από ένα κατάστημα δεδομένων εσωτερικής εγκατάστασης με χρήση της πύλης διαχείρισης δεδομένων. 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Πρόβλημα: On demand HDInsight προμήθεια αποτύχει
Όταν χρησιμοποιείτε ένα συνδεδεμένο της υπηρεσίας τύπου HDInsightOnDemand, πρέπει να καθορίσετε μια linkedServiceName που οδηγεί σε ένα χώρο αποθήκευσης Blob του Azure. Υπηρεσία εργοστασίου δεδομένων χρησιμοποιεί αυτό το χώρο αποθήκευσης για την αποθήκευση αρχείων καταγραφής και τα αρχεία υποστήριξης για το σύμπλεγμα HDInsight στη ζήτηση.  Μερικές φορές προμήθεια από ένα σύμπλεγμα HDInsight σε ζήτηση αποτυγχάνει με το ακόλουθο σφάλμα:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Αυτό το σφάλμα συνήθως υποδηλώνει ότι δεν είναι στη θέση του λογαριασμού χώρου αποθήκευσης που καθορίζεται στο το linkedServiceName στην ίδια θέση κέντρο δεδομένων όπου το HDInsight προμήθεια συμβαίνει. Παράδειγμα: Εάν εργοστασίου σας δεδομένων είναι στη δυτική η.π.α. και το Azure χώρου αποθήκευσης είναι στις Ανατολικής η.π.α., την προμήθεια του σε ζήτηση αποτυγχάνει στη δυτική η.π.α..

Επιπλέον, υπάρχει μια δεύτερη additionalLinkedServiceNames ιδιότητα JSON όπου λογαριασμούς επιπλέον χώρο αποθήκευσης ενδέχεται να έχει καθοριστεί σε HDInsight σε ζήτηση. Αυτούς τους λογαριασμούς επιπλέον χώρο αποθήκευσης συνδεδεμένων πρέπει να είναι στην ίδια θέση όπως το HDInsight σύμπλεγμα ή αποτύχει με το ίδιο σφάλμα.

### <a name="problem-custom-net-activity-fails"></a>Πρόβλημα: Προσαρμοσμένη δραστηριότητα .NET αποτυγχάνει
Για λεπτομερείς οδηγίες, ανατρέξτε στο θέμα [Εντοπισμός σφαλμάτων μια σωλήνωσης με προσαρμοσμένη δραστηριότητα](data-factory-use-custom-activities.md#debug-the-pipeline) . 

## <a name="use-azure-portal-to-troubleshoot"></a>Χρήση του Azure πύλη για την αντιμετώπιση προβλημάτων 

### <a name="using-portal-blades"></a>Χρήση πύλης πτερύγια
Για οδηγίες, ανατρέξτε στο θέμα [διοχέτευσης οθόνη](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) . 

### <a name="using-monitor-and-manage-app"></a>Χρησιμοποιώντας την εποπτεία και διαχείριση εφαρμογών
Ανατρέξτε στο θέμα [οθόνη και να διαχειριστείτε αγωγούς εργοστασίου δεδομένων με χρήση οθόνης και διαχείριση εφαρμογών](data-factory-monitor-manage-app.md) για λεπτομέρειες. 

## <a name="use-azure-powershell-to-troubleshoot"></a>Χρήση του PowerShell Azure για την αντιμετώπιση προβλημάτων

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Χρήση του PowerShell Azure για την αντιμετώπιση προβλημάτων σε ένα μήνυμα σφάλματος  
Για λεπτομέρειες, ανατρέξτε στο θέμα [αγωγούς οθόνη εργοστασίου δεδομένων με χρήση του PowerShell Azure](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) . 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 