<properties
    pageTitle="Μια νέα υπηρεσία web, χρησιμοποιώντας το cmdlet του PowerShell διαχείρισης μηχανικής εκμάθησης επαναρύθμιση | Microsoft Azure"
    description="Μάθετε πώς να μέσω προγραμματισμού Επαναρύθμιση μοντέλου και ενημέρωση της υπηρεσίας web για τη χρήση του μοντέλου που μόλις εκπαιδευμένο στο Azure μηχανικής εκμάθησης χρησιμοποιώντας τα cmdlet του PowerShell διαχείρισης μηχανικής εκμάθησης."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Επαναρύθμιση μια νέα υπηρεσία web, χρησιμοποιώντας το cmdlet του PowerShell διαχείρισης μηχανικής εκμάθησης

Όταν Επαναρύθμιση μια νέα υπηρεσία web, μπορείτε να ενημερώσετε τον ορισμό υπηρεσίας web πρόβλεψης για το νέο μοντέλο εκπαιδευμένο αναφορά.  

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Πρέπει να ορίσετε μια έρευνα εκπαίδευση και μια πρόβλεψης έρευνας όπως φαίνεται στα [μοντέλα μέσω προγραμματισμού Επαναρύθμιση μηχανικής εκμάθησης](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] Πρέπει να αναπτυχθούν το πρόβλεψης έρευνα ως ένα διαχειριστή πόρων Azure (νέα) που βασίζονται μηχανικής εκμάθησης υπηρεσία web. 
 
Για πρόσθετες πληροφορίες σχετικά με τις υπηρεσίες web ανάπτυξη, ανατρέξτε στο θέμα [ανάπτυξη μιας υπηρεσίας web Azure μηχανικής εκμάθησης](machine-learning-publish-a-machine-learning-web-service.md).

Αυτή η διαδικασία απαιτεί ότι έχετε εγκαταστήσει τα cmdlet Azure μηχανικής εκμάθησης. Για την εγκατάσταση του τα cmdlet μηχανικής εκμάθησης πληροφορίες, ανατρέξτε στο άρθρο αναφορά [Των cmdlet του Azure μηχανικής εκμάθησης](https://msdn.microsoft.com/library/azure/mt767952.aspx) στο MSDN.

Αντιγράψατε τις ακόλουθες πληροφορίες από το αποτέλεσμα του επαναπροσαρμογής:

* BaseLocation
* RelativeLocation

Τα βήματα που κάνετε είναι οι εξής:

1.  Πραγματοποιήστε είσοδο στο λογαριασμό σας για τη διαχείριση πόρων Azure.
2.  Λάβετε τον ορισμό υπηρεσίας web
3.  Εξαγωγή τον ορισμό υπηρεσίας Web ως JSON
4.  Ενημερώστε την αναφορά για το αντικείμενο blob ilearner στο το JSON.
5.  Εισαγωγή του JSON σε έναν ορισμό υπηρεσίας Web
6.  Ενημέρωση της υπηρεσίας web με το νέο ορισμό υπηρεσίας Web

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Πραγματοποιήστε είσοδο στο λογαριασμό σας για τη διαχείριση πόρων Azure

Πρέπει να εισέλθετε πρώτα Azure το λογαριασμό σας του περιβάλλοντος του PowerShell χρησιμοποιώντας το cmdlet [Προσθήκη AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition"></a>Λάβετε τον ορισμό υπηρεσίας Web

Στη συνέχεια, μπορείτε να λάβετε την υπηρεσία Web, καλώντας το cmdlet [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) . Τον ορισμό υπηρεσίας Web είναι μια εσωτερική αναπαράσταση του μοντέλου εκπαιδευμένο της υπηρεσίας web και δεν είναι δυνατό να τροποποιηθεί απευθείας. Βεβαιωθείτε ότι κάνετε ανάκτηση τον ορισμό της υπηρεσίας Web για το πρόβλεψης έρευνας και δεν σας έρευνας εκπαίδευσης.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Για να προσδιορίσετε το όνομα της ομάδας πόρων από μια υπάρχουσα υπηρεσία web, εκτελέστε το cmdlet Get-AzureRmMlWebService χωρίς παραμέτρους για να εμφανίσετε τις υπηρεσίες web στη συνδρομή σας. Εντοπίστε την υπηρεσία web και, στη συνέχεια, εξετάστε το αναγνωριστικό υπηρεσίας web Το όνομα της ομάδας πόρων είναι το τέταρτο στοιχείο στο πεδίο ID, απλώς μετά το στοιχείο *resourceGroups* . Στο παρακάτω παράδειγμα, το όνομα της ομάδας πόρων είναι προεπιλεγμένη-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Εναλλακτικά, για να προσδιορίσετε το όνομα της ομάδας πόρων από μια υπάρχουσα υπηρεσία web, συνδεθείτε στην πύλη του υπηρεσίες Web του Microsoft Azure μηχανικής εκμάθησης. Επιλέξτε την υπηρεσία web. Το όνομα της ομάδας πόρων είναι το πέμπτο στοιχείο της διεύθυνσης URL της υπηρεσίας web, απλώς μετά το στοιχείο *resourceGroups* . Στο παρακάτω παράδειγμα, το όνομα της ομάδας πόρων είναι προεπιλεγμένη-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Εξαγωγή τον ορισμό υπηρεσίας Web ως JSON

Για να τροποποιήσετε τον ορισμό στο μοντέλο εκπαιδευμένο για να χρησιμοποιήσετε το που μόλις εκπαιδευμένο μοντέλο, πρέπει πρώτα να χρησιμοποιήσετε το cmdlet [AzureRmMlWebService εξαγωγής](https://msdn.microsoft.com/library/azure/mt767935.aspx) για να εξαγάγετε σε ένα αρχείο σε μορφή JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Ενημερώστε την αναφορά για το αντικείμενο blob ilearner στο το JSON.

Των περιουσιακών στοιχείων, εντοπίστε την [εκπαιδευμένο μοντέλο], η ενημέρωση της τιμής *uri* στον κόμβο *locationInfo* με το URI της το blob ilearner. Το URI δημιουργείται συνδυάζοντας το *BaseLocation* και το *RelativeLocation* από την έξοδο από το ΕΙΚΟΝΊΔΙΟ επιμόρφωση κλήσης.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
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

## <a name="import-the-json-into-a-web-service-definition"></a>Εισαγωγή του JSON σε έναν ορισμό υπηρεσίας Web

Πρέπει να χρησιμοποιήσετε το cmdlet [AzureRmMlWebService εισαγωγή](https://msdn.microsoft.com/library/azure/mt767925.aspx) για να μετατρέψετε το τροποποιημένο αρχείο JSON ξανά σε έναν ορισμό υπηρεσίας Web που μπορείτε να χρησιμοποιήσετε για να ενημερώσετε την έρευνα Predicative.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Ενημέρωση της υπηρεσίας web με το νέο ορισμό υπηρεσίας Web

Τέλος, μπορείτε να χρησιμοποιήσετε το cmdlet [Ενημέρωση AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) για να ενημερώσετε το πρόβλεψης έρευνας.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Σύνοψη

Χρησιμοποιώντας το cmdlet διαχείρισης μηχανικής εκμάθησης PowerShell, μπορείτε να ενημερώσετε το μοντέλο εκπαιδευμένο πρόβλεψης υπηρεσίας Web Ενεργοποίηση σεναρίων, όπως:

* Περιοδικές μοντέλο επιμόρφωση με νέα δεδομένα.
* Η κατανομή ενός μοντέλου στους πελάτες με το στόχο επιτρέποντάς τους Επαναρύθμιση στο μοντέλο χρησιμοποιώντας τα δικά τους δεδομένα.
