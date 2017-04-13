<properties
    pageTitle="Επαναρύθμιση μια υπάρχουσα υπηρεσία web πρόβλεψης | Microsoft Azure"
    description="Μάθετε πώς να Επαναρύθμιση μοντέλου και ενημέρωση της υπηρεσίας web για τη χρήση του μοντέλου που μόλις εκπαιδευμένο στο Azure μηχανικής εκμάθησης."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Επαναρύθμιση μια υπάρχουσα υπηρεσία web πρόβλεψης

Αυτό το έγγραφο περιγράφει τη διαδικασία επαναπροσαρμογής για το ακόλουθο σενάριο:

* Έχετε μια έρευνα εκπαίδευση και μια πρόβλεψης έρευνας που έχουν αναπτυχθεί ως υπηρεσία operationalized web.
* Έχετε νέα δεδομένα που θέλετε για την υπηρεσία web πρόβλεψης για να χρησιμοποιήσετε για να εκτελέσετε τη βαθμολογία.

Ξεκινώντας με το υπάρχον υπηρεσίας web και δοκιμές, που πρέπει να ακολουθήσετε τα παρακάτω βήματα:

1. Ενημερώστε το μοντέλο.
    1. Τροποποίηση του εκπαιδευτικού έρευνας για να επιτρέψετε για εισόδων υπηρεσίας web και των εξόδων.
    2. Αναπτύξτε την έρευνα εκπαίδευση ως επαναπροσαρμογής υπηρεσία web.
    3. Χρησιμοποιήστε το έρευνας του εκπαιδευτικού δέσμη εκτέλεσης υπηρεσίας (BES) για να Επαναρύθμιση στο μοντέλο.
2. Χρησιμοποιήστε τα cmdlet Azure μηχανικής εκμάθησης PowerShell για να ενημερώσετε το πρόβλεψης έρευνας.
    1.  Πραγματοποιήστε είσοδο στο λογαριασμό σας για τη διαχείριση πόρων Azure.
    2.  Λάβετε τον ορισμό της υπηρεσίας web.
    3.  Εξαγάγετε τον ορισμό υπηρεσίας web ως JSON.
    4.  Ενημερώστε την αναφορά για το αντικείμενο blob ilearner στο το JSON.
    5.  Εισαγάγετε το JSON σε έναν ορισμό υπηρεσίας web.
    6.  Ενημέρωση της υπηρεσίας web με ένα νέο ορισμό υπηρεσίας web.

## <a name="deploy-the-training-experiment"></a>Ανάπτυξη του εκπαιδευτικού έρευνας

Για να αναπτύξετε την έρευνα εκπαίδευση ως επαναπροσαρμογής υπηρεσία web, πρέπει να προσθέσετε εισόδων υπηρεσίας web και των εξόδων του μοντέλου. Συνδέοντας μια λειτουργική μονάδα *Εξόδου υπηρεσίας Web* για να την έρευνα * [Μοντέλο τρένο] [ train-model] * λειτουργική μονάδα, ενεργοποιείτε την έρευνα εκπαίδευση για να δημιουργήσετε ένα νέο μοντέλο εκπαιδευμένο που μπορείτε να χρησιμοποιήσετε στο σας πρόβλεψης έρευνας. Εάν έχετε μια λειτουργική μονάδα *Αξιολόγηση μοντέλο* , μπορείτε επίσης να επισυνάψετε εξόδου υπηρεσίας web για να λάβετε τα αποτελέσματα του υπολογισμού ως αποτέλεσμα.

Για να ενημερώσετε την έρευνα εκπαίδευση:

1. Συνδέστε μια λειτουργική μονάδα *Υπηρεσίας Web εισαγωγής* για την εισαγωγή δεδομένων (για παράδειγμα, μια ενότητα *Clean δεδομένα λείπουν* ). Συνήθως, θέλετε να εξασφαλίσετε ότι τα δεδομένα εισόδου σας είναι υποβάλλονται σε επεξεργασία με τον ίδιο τρόπο όπως τα αρχικά δεδομένα εκπαίδευση.
2. Συνδέστε μια λειτουργική μονάδα *Εξόδου υπηρεσίας Web* για το αποτέλεσμα της λειτουργικής μονάδας *Τρένο μοντέλο* .
3. Εάν έχετε μια λειτουργική μονάδα *Αξιολόγηση μοντέλο* και θέλετε να εξαγάγετε τα αποτελέσματα του υπολογισμού, συνδέστε μια λειτουργική μονάδα *Εξόδου υπηρεσίας Web* για το αποτέλεσμα της λειτουργικής μονάδας *Αξιολόγηση μοντέλο* .

Εκτελέστε την έρευνα.

Στη συνέχεια, πρέπει να αναπτύξετε την εκπαίδευση έρευνας με μια υπηρεσία web που παράγει μοντέλου εκπαιδευμένο και μοντέλο αξιολόγησης αποτελέσματα.  

Στο κάτω μέρος του καμβά έρευνας, κάντε κλικ στην επιλογή **Ορισμός υπηρεσίας Web**και, στη συνέχεια, επιλέξτε **Ανάπτυξη υπηρεσίας Web [νέο]**. Η πύλη υπηρεσίες Web του Azure μηχανικής εκμάθησης ανοίγει στη σελίδα **Ανάπτυξη των υπηρεσιών Web** . Πληκτρολογήστε ένα όνομα για την υπηρεσία web, επιλέξτε ένα σχέδιο πληρωμής και, στη συνέχεια, κάντε κλικ στην επιλογή **Ανάπτυξη**. Μπορείτε να χρησιμοποιήσετε τη μέθοδο εκτέλεσης δέσμη μόνο για τη δημιουργία εκπαιδευμένο μοντέλα.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Επαναρύθμιση το μοντέλο με νέα δεδομένα χρησιμοποιώντας BES

Για αυτό το παράδειγμα, χρησιμοποιούμε τη C# για να δημιουργήσετε την εφαρμογή επαναπροσαρμογής. Μπορείτε επίσης να χρησιμοποιήσετε το δείγμα κώδικα Python ή R για να εκτελέσετε αυτήν την εργασία.

Για να καλέσετε τα API επαναπροσαρμογής:

1. Δημιουργία μιας εφαρμογής κονσόλας C# στο Visual Studio (**Δημιουργία** > **έργου** > **Επιφάνεια εργασίας των Windows** > **Εφαρμογής κονσόλας**).
2.  Είσοδος στην πύλη υπηρεσιών Web μηχανικής εκμάθησης.
3.  Κάντε κλικ στην υπηρεσία web που εργάζεστε με.
2.  Κάντε κλικ στην επιλογή **Χρήση**.
3.  Στο κάτω μέρος της σελίδας **ανάλωση** , στην ενότητα **Δείγμα κώδικα** , κάντε κλικ στην επιλογή **δέσμης**.
5.  Αντιγράψτε τον κώδικα C# δείγμα για εκτέλεση ενεργειών δέσμης και επικολλήστε το στο αρχείο Program.cs. Βεβαιωθείτε ότι ο χώρος ονομάτων παραμένει ανέπαφο.

Προσθέστε το πακέτο NuGet Microsoft.AspNet.WebApi.Client, όπως καθορίζεται στα σχόλια. Για να προσθέσετε την αναφορά σε Microsoft.WindowsAzure.Storage.dll, ενδέχεται να πρέπει πρώτα να εγκαταστήσετε τη [βιβλιοθήκη προγράμματος-πελάτη για τις υπηρεσίες αποθήκευσης Azure](https://www.nuget.org/packages/WindowsAzure.Storage).

Το παρακάτω στιγμιότυπο οθόνης εμφανίζει τη σελίδα **ανάλωση** στην πύλη του υπηρεσίες Web του Azure μηχανικής εκμάθησης.

![Εκμετάλλευση σελίδας][1]

### <a name="update-the-apikey-declaration"></a>Ενημερώστε τη δήλωση apikey

Εντοπίστε τη δήλωση **apikey** :

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Στην ενότητα **κατανάλωση βασικές πληροφορίες** από τη σελίδα **ανάλωση** , εντοπίστε το πρωτεύον κλειδί και αντιγράψτε την στη δήλωση **apikey** .

### <a name="update-the-azure-storage-information"></a>Ενημερώστε τις πληροφορίες της αποθήκευσης Azure

Το δείγμα κώδικα BES αποστέλλει ένα αρχείο από μια τοπική μονάδα δίσκου (για παράδειγμα, "C:\temp\CensusIpnput.csv") με το χώρο αποθήκευσης Azure, επεξεργάζεται και γράφει τα αποτελέσματα με το χώρο αποθήκευσης Azure.  

Για να ενημερώσετε τις πληροφορίες αποθήκευσης Azure, πρέπει να ανακτήσετε το όνομα του λογαριασμού χώρου αποθήκευσης, κλειδί και κοντέινερ πληροφορίες για το λογαριασμό χώρου αποθήκευσης από την πύλη του Azure κλασική και, στη συνέχεια, ενημέρωση του correspondi μετά την εκτέλεση του έρευνας, η ροή εργασίας που προκύπτει πρέπει να είναι παρόμοιο με το εξής:

![Που προκύπτει ροής εργασιών αφού ολοκληρωθεί η εκτέλεση][4]ση τιμές στον κώδικα.

1. Είσοδος στην πύλη του Azure κλασική.
1. Στη στήλη αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Αποθήκευση**.
1. Από τη λίστα λογαριασμών χώρου αποθήκευσης, επιλέξτε έναν για να αποθηκεύσετε το μοντέλο retrained.
1. Στο κάτω μέρος της σελίδας, κάντε κλικ στην επιλογή **Διαχείριση πλήκτρων πρόσβασης**.
1. Αντιγραφή και αποθηκεύσετε το **Πρωτεύον κλειδί πρόσβασης** και κλείστε το παράθυρο διαλόγου.
1. Στο επάνω μέρος της σελίδας, κάντε κλικ στο **κοντέινερ**.
1. Επιλέξτε ένα υπάρχον κοντέινερ, ή δημιουργήστε ένα νέο και αποθηκεύστε το όνομα.

Εντοπίστε το *StorageAccountName* *StorageAccountKey*και *StorageContainerName* δηλώσεις και ενημερώστε τις τιμές που έχετε αποθηκεύσει από την πύλη κλασική.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Πρέπει επίσης να εξασφαλίσετε ότι το αρχείο εισόδου είναι διαθέσιμες στη θέση που καθορίζετε τον κώδικα.

### <a name="specify-the-output-location"></a>Καθορίστε τη θέση εξόδου

Όταν καθορίζετε τη θέση εξόδου στο φορτίο αίτηση, την επέκταση του αρχείου που καθορίζεται στο *RelativeLocation* πρέπει να έχει καθοριστεί ως `ilearner`. Δείτε το παρακάτω παράδειγμα:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Ακολουθεί ένα παράδειγμα της επαναπροσαρμογής εξόδου: ![επιμόρφωση εξόδου][6]

## <a name="evaluate-the-retraining-results"></a>Αξιολογεί τα αποτελέσματα επαναπροσαρμογής

Όταν εκτελείτε την εφαρμογή, το αποτέλεσμα περιλαμβάνει τη διεύθυνση URL και διακριτικό υπογραφές κοινόχρηστο πρόσβασης που είναι απαραίτητα για να αποκτήσετε πρόσβαση τα αποτελέσματα του υπολογισμού.

Μπορείτε να δείτε τα αποτελέσματα επιδόσεων του μοντέλου retrained, συνδυάζοντας τις τιμές του *BaseLocation* *RelativeLocation*και *SasBlobToken* από τα αποτελέσματα εξόδου για *output2* (όπως φαίνεται στην προηγούμενη εικόνα επαναπροσαρμογής εξόδου) και επικολλώντας την πλήρη διεύθυνση URL στη γραμμή διευθύνσεων του προγράμματος περιήγησης.  

Εξετάστε τα αποτελέσματα για να προσδιορίσετε αν το μοντέλο που μόλις εκπαιδευμένο εκτελεί επίσης αρκετά για να αντικαταστήσετε το υπάρχον.

Αντιγράψτε το *BaseLocation* *RelativeLocation*και *SasBlobToken* από τα αποτελέσματα εξόδου.

## <a name="retrain-the-web-service"></a>Επαναρύθμιση υπηρεσίας web

Όταν Επαναρύθμιση μια νέα υπηρεσία web, μπορείτε να ενημερώσετε τον ορισμό υπηρεσίας web πρόβλεψης για το νέο μοντέλο εκπαιδευμένο αναφορά. Τον ορισμό υπηρεσίας web είναι μια εσωτερική αναπαράσταση του μοντέλου εκπαιδευμένο της υπηρεσίας web και δεν είναι δυνατό να τροποποιηθεί απευθείας. Βεβαιωθείτε ότι κάνετε ανάκτηση τον ορισμό της υπηρεσίας web για το πρόβλεψης έρευνας και δεν σας έρευνας εκπαίδευσης.

## <a name="sign-in-to-azure-resource-manager"></a>Είσοδος στο Azure από διαχειριστή πόρων

Πρέπει να εισέλθετε πρώτα Azure το λογαριασμό σας του περιβάλλοντος του PowerShell, χρησιμοποιώντας το cmdlet [Προσθήκη AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition-object"></a>Λήψη του αντικειμένου ορισμού υπηρεσίας Web

Στη συνέχεια, μπορείτε να λάβετε το αντικείμενο ορισμού υπηρεσίας Web καλώντας το cmdlet [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) .

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Για να προσδιορίσετε το όνομα της ομάδας πόρων από μια υπάρχουσα υπηρεσία web, εκτελέστε το cmdlet Get-AzureRmMlWebService χωρίς παραμέτρους για να εμφανίσετε τις υπηρεσίες web στη συνδρομή σας. Εντοπίστε την υπηρεσία web και, στη συνέχεια, εξετάστε το αναγνωριστικό υπηρεσίας web Το όνομα της ομάδας πόρων είναι το τέταρτο στοιχείο στο πεδίο ID, απλώς μετά το στοιχείο *resourceGroups* . Στο παρακάτω παράδειγμα, το όνομα της ομάδας πόρων είναι προεπιλεγμένη-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Εναλλακτικά, για να προσδιορίσετε το όνομα της ομάδας πόρων από μια υπάρχουσα υπηρεσία web, πραγματοποιήστε είσοδο στο την πύλη υπηρεσίες Web του Azure μηχανικής εκμάθησης. Επιλέξτε την υπηρεσία web. Το όνομα της ομάδας πόρων είναι το πέμπτο στοιχείο της διεύθυνσης URL της υπηρεσίας web, απλώς μετά το στοιχείο *resourceGroups* . Στο παρακάτω παράδειγμα, το όνομα της ομάδας πόρων είναι προεπιλεγμένη-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Εξαγάγετε το αντικείμενο ορισμού υπηρεσίας Web ως JSON

Για να τροποποιήσετε τον ορισμό του εκπαιδευμένο μοντέλου για να χρησιμοποιήσετε το μοντέλο που μόλις εκπαιδευμένο, πρέπει πρώτα να χρησιμοποιήσετε το cmdlet [AzureRmMlWebService εξαγωγής](https://msdn.microsoft.com/library/azure/mt767935.aspx) για να εξαγάγετε σε ένα αρχείο σε μορφή JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Ενημερώστε την αναφορά για το αντικείμενο blob ilearner

Των περιουσιακών στοιχείων, εντοπίστε την [εκπαιδευμένο μοντέλο], η ενημέρωση της τιμής *uri* στον κόμβο *locationInfo* με το URI της το blob ilearner. Το URI δημιουργείται συνδυάζοντας το *BaseLocation* και το *RelativeLocation* από την έξοδο από το ΕΙΚΟΝΊΔΙΟ επιμόρφωση κλήσης.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>Εισαγωγή του JSON σε ένα αντικείμενο ορισμού υπηρεσίας Web

Πρέπει να χρησιμοποιήσετε το cmdlet [AzureRmMlWebService εισαγωγή](https://msdn.microsoft.com/library/azure/mt767925.aspx) για να μετατρέψετε το τροποποιημένο αρχείο JSON πίσω σε ένα αντικείμενο ορισμού υπηρεσίας Web που μπορείτε να χρησιμοποιήσετε για να ενημερώσετε το predicative έρευνας.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Ενημέρωση της υπηρεσίας web

Τέλος, μπορείτε να χρησιμοποιήσετε το cmdlet [AzureRmMlWebService ενημέρωσης](https://msdn.microsoft.com/library/azure/mt767922.aspx) για να ενημερώσετε το πρόβλεψης έρευνας.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
