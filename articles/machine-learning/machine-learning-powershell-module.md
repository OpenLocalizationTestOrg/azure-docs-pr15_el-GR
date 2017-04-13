<properties
    pageTitle="Λειτουργική μονάδα PowerShell για μηχανικής εκμάθησης | Microsoft Azure"
    description="Η λειτουργική μονάδα PowerShell για Azure μηχανικής εκμάθησης είναι διαθέσιμη σε κατάσταση προεπισκόπησης δημόσια. Χρήση του PowerShell για να δημιουργήσετε και να διαχειριστείτε τους χώρους εργασίας, δοκιμές, υπηρεσίες web και άλλα."
    keywords="Πειραματιστείτε, γραμμικής παλινδρόμησης, μηχανικής εκμάθησης αλγόριθμοι, μηχανικής εκμάθησης το πρόγραμμα εκμάθησης, τεχνικές πρόβλεψης μοντελοποίηση, δεδομένα science έρευνας"
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
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>Λειτουργική μονάδα του PowerShell για το Microsoft Azure μηχανικής εκμάθησης

Η λειτουργική μονάδα PowerShell για Azure μηχανικής εκμάθησης είναι ένα ισχυρό εργαλείο που σας επιτρέπει να χρησιμοποιείτε το Windows PowerShell για τη Διαχείριση χώρων εργασίας, δοκιμές, σύνολα δεδομένων, web serivces και άλλα.

Μπορείτε να προβάλετε την τεκμηρίωση και να κάνετε λήψη της λειτουργικής μονάδας, μαζί με το πλήρες πηγαίου κώδικα, στο [https://aka.ms/amlps](https://aka.ms/amlps). 

## <a name="what-is-the-machine-learning-powershell-module"></a>Τι είναι το της λειτουργικής μονάδας μηχανικής εκμάθησης PowerShell;

Η λειτουργική μονάδα μηχανικής εκμάθησης PowerShell είναι ένα. Βάσει Καθαρής DLL λειτουργική μονάδα που σας επιτρέπει να πλήρως Διαχείριση χώρων εργασίας Azure μηχανικής εκμάθησης, δοκιμές, σύνολα δεδομένων, υπηρεσίες web και τελικά σημεία υπηρεσίας web από το Windows PowerShell. Μαζί με τη λειτουργική μονάδα, μπορείτε να κάνετε λήψη του πλήρους πηγαίου κώδικα που περιλαμβάνει ένα [επίπεδο C# API](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs)καθαρού διαχωρισμένο με. Αυτό σημαίνει ότι μπορείτε να αναφέρονται σε αυτό το DLL από το δικό σας έργο .NET και να διαχειριστείτε Azure μηχανικής εκμάθησης μέσω .NET κώδικα. Επιπλέον, το DLL εξαρτάται από υποκείμενο REST API τα οποία μπορείτε να αξιοποιήσετε απευθείας από το πρόγραμμα-πελάτη Αγαπημένα.

## <a name="what-can-i-do-with-the-powershell-module"></a>Τι μπορώ να κάνω με τη λειτουργική μονάδα PowerShell;

Εδώ θα βρείτε ορισμένες από τις εργασίες που μπορείτε να εκτελέσετε με αυτήν τη λειτουργική μονάδα PowerShell. Δείτε την [πλήρη τεκμηρίωση](https://aka.ms/amlps) για αυτές και πολλές περισσότερες συναρτήσεις.

- Προμήθεια νέου χώρου εργασίας χρησιμοποιώντας ένα πιστοποιητικό διαχείρισης ([Δημιουργία AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
- Εξαγωγή και εισαγωγή ενός αρχείου JSON που αντιπροσωπεύει ένα γράφημα έρευνας ([AmlExperimentGraph εξαγωγή](https://github.com/hning86/azuremlps#export-amlexperimentgraph) και [Εισαγωγή AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
- Εκτέλεση μιας έρευνας ([Έναρξη-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
- Δημιουργήστε μια υπηρεσία web από μια πρόβλεψης έρευνας ([Δημιουργία AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Δημιουργήστε ένα νέο τελικό σημείο σε μια υπηρεσία web που έχει δημοσιευθεί ([Προσθήκη AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Κλήση μιας στο ή/και BES web τελικού σημείου υπηρεσίας ([Ενεργοποίηση AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) και [Ενεργοποίηση AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Ακολουθεί ένα γρήγορο παράδειγμα της χρήσης του PowerShell για να εκτελέσετε μια υπάρχουσα έρευνα:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Για μια πιο αναλυτικά περίπτωσης χρήσης, ανατρέξτε στο άρθρο σχετικά με τη χρήση της λειτουργικής μονάδας PowerShell για την αυτοματοποίηση μιας εργασίας πολύ συχνά ζητήθηκε: [Δημιουργία πολλά μοντέλα μηχανικής εκμάθησης και web τελικά σημεία υπηρεσίας από τη χρήση του PowerShell έρευνα](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Πώς μπορώ να ξεκινήσω;

Για να ξεκινήσετε με το PowerShell εκμάθησης υπολογιστή, κάντε λήψη του [αφήσετε το πακέτο](https://github.com/hning86/azuremlps/releases) από GitHub και ακολουθήστε τις [οδηγίες για την εγκατάσταση](https://github.com/hning86/azuremlps/blob/master/README.md). Θα πρέπει να καταργήσετε τον αποκλεισμό του DLL ληφθεί/αποσυμπιεσμένο αρχείο και, στη συνέχεια, εισαγάγετέ το στο περιβάλλον του PowerShell. Περισσότερα από τα cmdlet απαιτούν που δίνετε το Αναγνωριστικό του χώρου εργασίας, το διακριτικό εξουσιοδότησης χώρου εργασίας και το Azure περιοχή οποία βρίσκεται στο χώρο εργασίας. Ο πιο απλός τρόπος για την παροχή αυτές είναι μέσω ένα προεπιλεγμένο αρχείο config.json, η οποία καλύπτεται με λεπτομέρειες σε τις οδηγίες εγκατάστασης. Φυσικά, μπορείτε επίσης κλωνοποίηση του δέντρου git και τροποποίηση και μεταγλώττιση του κώδικα τοπικά χρήση του Visual Studio.

## <a name="next-steps"></a>Επόμενα βήματα

Της λειτουργικής μονάδας PowerShell, θα εξακολουθήσει να είναι βελτιωμένη και έχει επεκταθεί κατά τη διάρκεια αυτής της περιόδου preview. Να παρακολουθείτε την [Cortana πληροφοριών και μηχανικής εκμάθησης ιστολογίων](https://blogs.technet.microsoft.com/machinelearning/) για περισσότερες πληροφορίες και συζητήσεων.
