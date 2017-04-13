<properties
pageTitle="Δημιουργία πολλών μοντέλα από έρευνα | Microsoft Azure"
description="Χρήση του PowerShell για τη δημιουργία πολλών μηχανικής εκμάθησης μοντέλων και web τελικά σημεία υπηρεσίας με το ίδιο αλγόριθμο αλλά εκπαίδευση διαφορετικά σύνολα δεδομένων."
services="machine-learning"
documentationCenter=""
authors="hning86"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="garye;haining"/>

# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Δημιουργία πολλά μοντέλα μηχανικής εκμάθησης και web τελικά σημεία υπηρεσίας από έρευνα χρήση του PowerShell

Ακολουθεί ένα κοινό θέμα μηχανικής εκμάθησης: που θέλετε να δημιουργήσετε πολλά μοντέλα που έχουν την ίδια ροή εργασίας εκπαίδευση και χρησιμοποιήστε τον ίδιο αλγόριθμο, αλλά έχετε εκπαίδευση διαφορετικά σύνολα δεδομένων ως είσοδο. Σε αυτό το άρθρο θα μάθετε πώς να το κάνετε αυτό με κλίμακα στο Azure μηχανικής εκμάθησης Studio χρησιμοποιώντας απλώς ένα μεμονωμένο έρευνας.

Για παράδειγμα, ας υποθέσουμε ότι είστε ο κάτοχος μιας επιχείρησης καθολικού ποδηλάτων ενοικίασης δικαιόχρησης. Θέλετε να δημιουργήσετε ένα μοντέλο παλινδρόμησης πρόβλεψη η μίσθωση ζήτηση που βασίζονται σε δεδομένα ιστορικού. Έχετε 1.000 ενοικίασης θέσεις σε ολόκληρο τον κόσμο και που έχετε συγκεντρώσει ένα σύνολο δεδομένων για κάθε τοποθεσία που περιλαμβάνει σημαντικές δυνατότητες όπως η ημερομηνία, ώρα, καιρού και την κυκλοφορία που είναι ειδικές για κάθε θέση.

Ενδέχεται να μπορείτε να εκπαιδεύσετε μοντέλου σας με μία φορά συγχωνευμένων έκδοση όλα τα σύνολα δεδομένων σε όλες τις θέσεις. Αλλά επειδή κάθε μία από τις θέσεις σας έχει ένα μοναδικό περιβάλλον, θα ήταν μια καλύτερη προσέγγιση για την εκπαίδευση του μοντέλου παλινδρόμησης ξεχωριστά χρησιμοποιώντας το σύνολο δεδομένων για κάθε θέση. Με αυτόν τον τρόπο, κάθε εκπαιδευμένο μοντέλο μπορεί να λαμβάνει υπόψη, τα μεγέθη διαφορετικό χώρο αποθήκευσης, ένταση, Γεωγραφία, πληθυσμού, κίνηση ποδηλάτων φιλικό περιβάλλον, *κ.λπ.*.

Που μπορεί να είναι η καλύτερη προσέγγιση, αλλά δεν θέλετε να δημιουργήσετε 1.000 δοκιμές εκπαίδευση στο Azure μηχανικής εκμάθησης με κάθε μία που αντιπροσωπεύει μια μοναδική θέση. Εκτός από τη υπερβολική εργασία του, είναι επίσης φαίνεται πολύ αποτελεσματική δεδομένου ότι κάθε έρευνας θα έχουν τα ίδια στοιχεία εκτός από το σύνολο δεδομένων εκπαίδευση.

Ευτυχώς, θα σας μπορεί να γίνει αυτό χρησιμοποιώντας το [Azure μηχανικής εκμάθησης επιμόρφωση API](machine-learning-retrain-models-programmatically.md) και αυτοματοποίηση την εργασία με το [Azure μηχανικής εκμάθησης PowerShell](machine-learning-powershell-module.md).

> [AZURE.NOTE] Για να μας δείγμα εκτελείται πιο γρήγορα, θα μπορούμε να μειώσουμε τον αριθμό των θέσεων από 1.000 σε 10. Αλλά το ίδιο αρχές και διαδικασίες εφαρμόζονται σε 1.000 θέσεις. Η μόνη διαφορά είναι ότι εάν θέλετε να εκπαιδεύσετε από 1.000 συνόλων δεδομένων πιθανώς θέλετε να πιστεύετε ότι τα ακόλουθα δέσμης ενεργειών του PowerShell που εκτελούνται παράλληλα. Πώς μπορείτε να το κάνετε είναι πέρα από το πεδίο αυτού του άρθρου, αλλά μπορείτε να βρείτε παραδείγματα PowerShell η πολυνηματική επεξεργασία στο Internet.  

## <a name="set-up-the-training-experiment"></a>Ρύθμιση του εκπαιδευτικού έρευνας

Θα σας πρόκειται να χρησιμοποιήσετε ένα παράδειγμα [πειραματιστείτε εκπαίδευσης](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) που έχουμε ήδη δημιουργήσει στη [Συλλογή πληροφοριών Cortana](http://gallery.cortanaintelligence.com). Ανοίξτε αυτό έρευνας στο χώρο εργασίας σας [Azure μηχανικής εκμάθησης Studio](https://studio.azureml.net) .

>[AZURE.NOTE] Για να ακολουθήσετε μαζί με αυτό το παράδειγμα, μπορεί να θέλετε να χρησιμοποιήσετε μια τυπική χώρου εργασίας αντί για ένα ελεύθερο χώρο εργασίας. Θα σας θα δημιουργήσετε ένα τελικό σημείο για κάθε πελάτη - για ένα σύνολο τελικά σημεία 10 - και που θα απαιτούν χώρου εργασίας τυπική δεδομένου ότι χώρου εργασίας δωρεάν περιορίζεται σε 3 τελικά σημεία. Εάν έχετε μόνο ελεύθερου χώρου εργασίας, μόλις τροποποιήσετε τις δέσμες ενεργειών παρακάτω, για να μην επιτρέπεται μόνο 3 θέσεις.

Η έρευνα χρησιμοποιεί μια λειτουργική μονάδα **Εισαγωγής δεδομένων** για να εισαγάγετε το σύνολο δεδομένων εκπαίδευση *customer001.csv* από ένα λογαριασμό Azure χώρου αποθήκευσης. Ας υποθέσουμε ότι θα σας έχετε συγκεντρώσει εκπαίδευση συνόλων δεδομένων από όλες τις θέσεις ενοικίασης ποδηλάτων και αποθηκεύονται στην ίδια θέση αποθήκευσης αντικειμένων blob με ονόματα αρχείων μεταξύ του *rentalloc001.csv* *rentalloc10.csv*.

![εικόνα](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Σημειώστε ότι μια λειτουργική μονάδα **Εξόδου υπηρεσίας Web** έχει προστεθεί στη λειτουργική μονάδα **Μοντέλο τρένο** .
Όταν αυτό έρευνας αναπτύσσεται ως μια υπηρεσία web, το τελικό σημείο που σχετίζεται με ότι εξόδου θα επιστρέψει το μοντέλο εκπαιδευμένο στη μορφή αρχείου .ilearner.

Επίσης, σημειώστε ότι μπορούμε να εγκαταστήσουμε παραμέτρου υπηρεσίας web για τη διεύθυνση URL που χρησιμοποιεί τη λειτουργική μονάδα **Εισαγωγή δεδομένων** . Αυτό σας επιτρέπει να χρησιμοποιήσετε την παράμετρο για να καθορίσετε μεμονωμένους εκπαίδευση σύνολα δεδομένων για την εκπαίδευση του μοντέλου για κάθε θέση.
Υπάρχουν άλλοι τρόποι που θα σας μπορεί να γίνει αυτό, όπως χρησιμοποιώντας ένα ερώτημα SQL με μια παράμετρο υπηρεσίας web για τη λήψη δεδομένων από μια βάση δεδομένων SQL Azure ή απλώς χρησιμοποιώντας μια λειτουργική μονάδα **Εισόδου δεδομένων υπηρεσίας Web** για τη μεταβίβαση σε ένα σύνολο δεδομένων για την υπηρεσία web.

![εικόνα](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Τώρα, ας εκτέλεση αυτό έρευνας εκπαίδευση χρησιμοποιώντας την προεπιλεγμένη τιμή *rental001.csv* ως του συνόλου δεδομένων εκπαίδευσης. Εάν προβάλλετε το αποτέλεσμα της λειτουργικής μονάδας **αξιολόγηση** (κάντε κλικ στην επιλογή την έξοδο και επιλέξτε **αναπαράσταση**), μπορείτε να δείτε λαμβάνουμε μια ικανοποιητική απόδοση της *AUC* = 0,91. Σε αυτό το σημείο, είστε έτοιμοι να αναπτύξετε μια υπηρεσία web από αυτό έρευνας εκπαίδευσης.

## <a name="deploy-the-training-and-scoring-web-services"></a>Ανάπτυξη της εκπαίδευση και βαθμολογίας υπηρεσίες web

Για να αναπτύξετε την υπηρεσία web εκπαίδευση, κάντε κλικ στο κουμπί **Ορισμός υπηρεσίας Web** κάτω από τον καμβά έρευνας και επιλέξτε **Ανάπτυξη υπηρεσία Web**. Κλήση αυτήν την υπηρεσία web "" ποδηλάτων ενοικίασης εκπαίδευση".

Τώρα πρέπει να αναπτύξετε την βαθμολογίας υπηρεσία web.
Για να το κάνετε αυτό, θα σας μπορεί να κάντε κλικ στην επιλογή **Ορισμός υπηρεσίας Web** κάτω από τον καμβά και επιλέξτε **Πρόβλεψης υπηρεσία Web**. Αυτό δημιουργεί μια βαθμολογίας έρευνας.
Θα πρέπει να κάνετε ορισμένες ρυθμίσεις μικρή ώστε να λειτουργεί ως μια υπηρεσία web, όπως η ετικέτα στήλης "cnt" Κατάργηση από τα δεδομένα εισόδου και τον περιορισμό την έξοδο σε μόνο το αναγνωριστικό εμφάνισης και το αντίστοιχο προβλεπόμενης τιμής.

Για να αποθηκεύσετε αυτή την εργασία, μπορείτε απλώς να ανοίξετε [πρόβλεψης έρευνας](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) στη συλλογή που έχει ήδη προετοιμαστεί.

Για να αναπτύξετε την υπηρεσία web, εκτελέστε το πρόβλεψης έρευνας και, στη συνέχεια, κάντε κλικ στο κουμπί **Ανάπτυξη υπηρεσίας Web** κάτω από τον καμβά. Βαθμολογίας υπηρεσίας web "Βαθμολογίας ενοικίασης ποδηλάτων" όνομα ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Δημιουργία 10 τελικά σημεία υπηρεσίας web όμοια με το PowerShell

Αυτή η υπηρεσία web συνοδεύεται από ένα προεπιλεγμένο τελικό σημείο. Αλλά θα σας δεν ενδιαφέρεστε ως το προεπιλεγμένο τελικό σημείο επειδή δεν είναι δυνατό να ενημερωθούν. Τι πρέπει να κάνετε είναι να δημιουργήσετε 10 επιπλέον τελικά σημεία, μία για κάθε θέση. Θα κάνουμε αυτό με το PowerShell.

Πρώτα, μπορούμε να εγκαταστήσουμε το περιβάλλον του PowerShell:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Στη συνέχεια, εκτελέστε την ακόλουθη εντολή PowerShell:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Τώρα που έχετε δημιουργήσαμε 10 τελικά σημεία και όλες περιέχουν στο ίδιο μοντέλο εκπαιδευμένο εκπαίδευση στις *customer001.csv*. Μπορείτε να τα προβάλετε στην πύλη διαχείρισης του Azure.

![εικόνα](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Ενημερώστε τα τελικά σημεία για να χρησιμοποιήσετε ξεχωριστό εκπαίδευση συνόλων δεδομένων με χρήση του PowerShell

Το επόμενο βήμα είναι να ενημερώσετε τα τελικά σημεία με τα μοντέλα με μοναδικό τρόπο εκπαίδευση στις μεμονωμένα δεδομένα κάθε πελάτη. Αλλά πρώτα πρέπει να παράγουν αυτά τα μοντέλα από την υπηρεσία web **Ποδηλάτων ενοικίασης εκπαίδευση** . Επιστρέψτε στο η υπηρεσία web **Ποδηλάτων ενοικίασης εκπαίδευσης** . Πρέπει να καλέσετε το τελικό σημείο BES 10 φορές με 10 εκπαίδευση διαφορετικά σύνολα δεδομένων προκειμένου να παραγάγετε 10 διαφορετικά μοντέλα. Θα χρησιμοποιήσουμε το cmdlet του PowerShell **InovkeAmlWebServiceBESEndpoint** να το κάνετε αυτό.

Θα πρέπει επίσης να δώσετε τα διαπιστευτήρια για το λογαριασμό σας χώρο αποθήκευσης αντικειμένων blob σε `$configContent`, δηλαδή, στα πεδία `AccountName`, `AccountKey` και `RelativeLocation`. Το `AccountName` μπορεί να είναι ένα από τα ονόματα λογαριασμού, όπως φαίνεται στην **Πύλη διαχείρισης κλασική Azure** (καρτέλα*χώρο αποθήκευσης* ). Αφού κάνετε κλικ σε ένα λογαριασμό του χώρου αποθήκευσης, το `AccountKey` μπορείτε να βρείτε, πατώντας το κουμπί **Διαχείριση πλήκτρα πρόσβασης** στο κάτω μέρος και αντιγραφή του *Πρωτεύοντος κλειδιού πρόσβασης*. Η `RelativeLocation` είναι η διαδρομή σε σχέση με το χώρο αποθήκευσης όπου θα αποθηκευτεί ένα νέο μοντέλο. Για παράδειγμα, η διαδρομή `hai/retrain/bike_rental/` στη δέσμη ενεργειών κάτω από το στοιχείο σημεία σε ένα κοντέινερ που ονομάζεται `hai`, και `/retrain/bike_rental/` είναι υποφάκελοι. Προς το παρόν, δεν μπορείτε να δημιουργήσετε υποφακέλους μέσω της πύλης περιβάλλοντος εργασίας Χρήστη, αλλά υπάρχουν [Αρκετές εξερεύνησης χώρου αποθήκευσης Azure](../storage/storage-explorers.md) που σας επιτρέπουν να το κάνετε. Καλό είναι να δημιουργήσετε ένα νέο κοντέινερ στο χώρο αποθήκευσης του για να αποθηκεύσετε το νέο εκπαιδευμένο μοντέλα (αρχεία .ilearner) ως εξής: από τη σελίδα χώρου αποθήκευσης, κάντε κλικ στο κουμπί " **Προσθήκη** " στο κάτω μέρος και ονομάστε το `retrain`. Εν κατακλείδι, τις απαιτούμενες αλλαγές για την παρακάτω δέσμη ενεργειών που σχετίζονται με `AccountName`, `AccountKey` και `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

>[AZURE.NOTE] Το τελικό σημείο BES είναι τα υποστηριζόμενα λειτουργία μόνο για αυτήν τη λειτουργία. ΣΤΟ δεν μπορούν να χρησιμοποιηθούν για την παραγωγή εκπαιδευμένο μοντέλα.

Όπως μπορείτε να δείτε παραπάνω, αντί η κατασκευή 10 διαφορετικά BES εργασία ρύθμισης παραμέτρων json αρχεία, θα σας δυναμικά Δημιουργήστε τη συμβολοσειρά ρύθμισης παραμέτρων αντί για αυτό και τροφοδοσίας για την παράμετρο *jobConfigString* από το cmdlet **InvokeAmlWebServceBESEndpoint** , επειδή δεν είναι στην πραγματικότητα χωρίς να χρειάζεται να διατηρήσετε ένα αντίγραφο στο δίσκο.

Εάν όλα τα στοιχεία μεταβαίνει σωστά, μετά από ένα χρονικό διάστημα θα πρέπει να βλέπετε αρχεία 10 .ilearner, από *model001.ilearner* σε *model010.ilearner*, στο λογαριασμό σας στο Azure χώρου αποθήκευσης. Τώρα είστε έτοιμοι να ενημερώσετε μας 10 βαθμολογίας web υπηρεσίας τελικά σημεία με αυτά τα μοντέλα χρησιμοποιώντας το cmdlet του PowerShell **AmlWebServiceEndpoint κώδικα** . Να θυμάστε ότι ξανά ότι θα σας μπορεί να κώδικα μόνο τα τελικά σημεία μη προεπιλεγμένες δημιουργήσαμε μέσω προγραμματισμού νωρίτερα.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Αυτό πρέπει να εκτελέσετε αρκετά γρήγορα. Όταν ολοκληρωθεί η εκτέλεση, θα με επιτυχία δημιουργήσαμε 10 πρόβλεψης web υπηρεσίας τελικά σημεία, μοντέλου εκπαιδευμένο με μοναδικό τρόπο εκπαίδευση στις το συγκεκριμένο σύνολο δεδομένων σε μια θέση ενοικίασης, όλες από ένα μεμονωμένο εκπαίδευση έρευνας που περιέχουν. Για να το επαληθεύσετε, μπορείτε να δοκιμάσετε κλήση αυτά τα τελικά σημεία χρησιμοποιώντας το cmdlet **InvokeAmlWebServiceRRSEndpoint** , παρέχοντας τους με τα ίδια δεδομένα εισόδου και θα πρέπει να περιμένετε για να δείτε διαφορετικές πρόβλεψη αποτελέσματα, επειδή τα μοντέλα είναι εκπαίδευση με διαφορετικό εκπαίδευση σύνολα.

## <a name="full-powershell-script"></a>Πλήρης δέσμη ενεργειών του PowerShell

Ακολουθεί η καταχώρηση του κώδικα πλήρους προέλευσης:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
