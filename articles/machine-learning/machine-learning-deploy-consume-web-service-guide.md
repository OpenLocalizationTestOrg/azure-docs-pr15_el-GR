<properties
    pageTitle="Υπηρεσίες Web εκμάθησης Azure υπολογιστή: Ανάπτυξη και κατανάλωση | Microsoft Azure"
    description="Πόροι για την ανάπτυξη και κατανάλωση υπηρεσιών web."
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
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure μηχανικής εκμάθησης υπηρεσίες Web του: Ανάπτυξη και κατανάλωση

Μπορείτε να χρησιμοποιήσετε το Azure μηχανικής εκμάθησης για να αναπτύξετε τις ροές εργασίας και τα μοντέλα με τις υπηρεσίες web μηχανικής εκμάθησης. Στη συνέχεια, μπορεί να χρησιμοποιηθεί αυτές τις υπηρεσίες web για να καλέσετε τα μοντέλα μηχανικής εκμάθησης από εφαρμογές μέσω του Internet για να το κάνετε προβλέψεων σε πραγματικό χρόνο ή σε λειτουργία δέσμης. Επειδή οι υπηρεσίες web είναι RESTful, μπορείτε να καλέσετε τους από διάφορες προγραμματισμού γλώσσες και πλατφόρμες, όπως το .NET και Java, και από τις εφαρμογές, όπως το Excel.

Οι επόμενες ενότητες παρέχουν συνδέσεις για αναλυτικές παρουσιάσεις, κώδικα και την τεκμηρίωση για να σας βοηθήσουν να ξεκινήσετε.

## <a name="deploy-a-web-service"></a>Ανάπτυξη μιας υπηρεσίας web

### <a name="with-azure-machine-learning-studio"></a>Με το Azure μηχανικής εκμάθησης Studio

Μηχανικής εκμάθησης Studio και της πύλης υπηρεσίες Web του Microsoft Azure μηχανικής εκμάθησης σας βοηθήσει να αναπτύξετε και να διαχειριστείτε μια υπηρεσία web χωρίς να συντάξετε κώδικα.

Οι ακόλουθες συνδέσεις παρέχουν γενικές πληροφορίες σχετικά με τον τρόπο για να αναπτύξετε μια νέα υπηρεσία web:

* Για μια επισκόπηση σχετικά με τον τρόπο για να αναπτύξετε μια νέα υπηρεσία web που βασίζεται στη Διαχείριση πόρων Azure, ανατρέξτε στο θέμα [Ανάπτυξη νέας υπηρεσίας web](machine-learning-webservice-deploy-a-web-service.md).
* Για αναλυτικές οδηγίες σχετικά με την ανάπτυξη μιας υπηρεσίας web, ανατρέξτε στο θέμα [ανάπτυξη μιας υπηρεσίας web Azure μηχανικής εκμάθησης](machine-learning-publish-a-machine-learning-web-service.md).
* Για πλήρεις αναλυτικές οδηγίες σχετικά με το πώς μπορείτε να δημιουργήσετε και να αναπτύξετε μια υπηρεσία web, ανατρέξτε στο θέμα [αναλυτικές οδηγίες βήμα 1: δημιουργία χώρου εργασίας μηχανικής εκμάθησης](machine-learning-walkthrough-1-create-ml-workspace.md).
* Για συγκεκριμένα παραδείγματα που αναπτύξετε μια υπηρεσία web, ανατρέξτε στα θέματα:

    * [Αναλυτικές οδηγίες βήμα 5: Ανάπτυξη της υπηρεσίας web Azure μηχανικής εκμάθησης](machine-learning-walkthrough-5-publish-web-service.md)
    * [Πώς μπορείτε να αναπτύξετε μια υπηρεσία web σε πολλές περιοχές](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Με το web πόρων της υπηρεσίας παροχής υπηρεσιών APIs (Azure API διαχείρισης πόρων)

Η υπηρεσία παροχής πόρων Azure μηχανικής εκμάθησης για τις υπηρεσίες web επιτρέπει ανάπτυξης και διαχείρισης των υπηρεσιών web, χρησιμοποιώντας τις κλήσεις REST API. Για περισσότερες λεπτομέρειες, ανατρέξτε στο άρθρο αναφορά [Υπηρεσίας Web εκμάθησης υπολογιστή (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) στο MSDN.

### <a name="with-powershell-cmdlets"></a>Με το cmdlet του PowerShell

Azure υπηρεσίας παροχής πόρων μηχανικής εκμάθησης για τις υπηρεσίες web επιτρέπει ανάπτυξης και διαχείρισης των υπηρεσιών web με χρήση των cmdlet του PowerShell.

Για να χρησιμοποιήσετε τα cmdlet, που πρέπει να είσοδο για πρώτη φορά Azure το λογαριασμό σας του περιβάλλοντος PowerShell, χρησιμοποιώντας το cmdlet [Προσθήκη AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) . Εάν είστε εξοικειωμένοι με το πώς μπορείτε να καλέσετε τις εντολές του PowerShell που βασίζονται στη Διαχείριση πόρων, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Για να εξαγάγετε σας πρόβλεψης έρευνας, χρησιμοποιήστε [το δείγμα κώδικα](https://github.com/ritwik20/AzureML-WebServices). Αφού δημιουργήσετε το αρχείο .exe από τον κώδικα, μπορείτε να πληκτρολογήσετε:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Εκτέλεση της εφαρμογής δημιουργεί ένα πρότυπο JSON υπηρεσία web. Για να χρησιμοποιήσετε το πρότυπο για να αναπτύξετε μια υπηρεσία web, πρέπει να προσθέσετε τις ακόλουθες πληροφορίες:

* Όνομα λογαριασμού χώρου αποθήκευσης και αριθμού-κλειδιού

    Μπορείτε να λάβετε το όνομα του λογαριασμού χώρου αποθήκευσης και το κλειδί από την [πύλη του Azure](https://portal.azure.com/) ή του [Azure κλασική πύλη](http://manage.windowsazure.com/).
* Αναγνωριστικό πρόγραμμα δέσμευσης

    Μπορείτε να λάβετε το Αναγνωριστικό πρόγραμμα από την πύλη [Υπηρεσίες Web του Azure μηχανικής εκμάθησης](https://services.azureml.net) είσοδος και κάνοντας κλικ σε ένα όνομα του προγράμματός σας.

Προσθέστε τις στο πρότυπο JSON ως θυγατρικά στοιχεία του κόμβου *Ιδιότητες* στο ίδιο επίπεδο με τον κόμβο *MachineLearningWorkspace* .

Ακολουθεί ένα παράδειγμα:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Ανατρέξτε στα παρακάτω άρθρα και δείγματα κώδικα για περισσότερες λεπτομέρειες:

* Αναφορά [Των cmdlet του Azure μηχανικής εκμάθησης]( https://msdn.microsoft.com/library/azure/mt767952.aspx) στο MSDN
* Δείγμα [αναλυτικές οδηγίες](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) σε GitHub

## <a name="consume-the-web-services"></a>Χρήση των υπηρεσιών web

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Από τις υπηρεσίες Web του εκμάθησης Azure υπολογιστή περιβάλλοντος εργασίας Χρήστη (δοκιμές)

Μπορείτε να δοκιμάσετε την υπηρεσία web από την πύλη υπηρεσίες Web του Azure μηχανικής εκμάθησης. Περιλαμβάνονται και οι δοκιμές την υπηρεσία αίτησης-απάντησης (στο) και διασυνδέσεις δέσμη εκτέλεσης υπηρεσίας (BES).

* [Ανάπτυξη μιας νέας υπηρεσίας web](machine-learning-webservice-deploy-a-web-service.md)
* [Ανάπτυξη μιας υπηρεσίας web Azure μηχανικής εκμάθησης](machine-learning-publish-a-machine-learning-web-service.md)
* [Αναλυτικές οδηγίες βήμα 5: Ανάπτυξη της υπηρεσίας web Azure μηχανικής εκμάθησης](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Από το Excel

Μπορείτε να κάνετε λήψη σε ένα πρότυπο του Excel που χρησιμοποιεί την υπηρεσία web:

* [Κατανάλωση μιας υπηρεσίας web Azure μηχανικής εκμάθησης από το Excel](machine-learning-consuming-from-excel.md)
* [Πρόσθετο του Excel για τις υπηρεσίες Web του Azure μηχανικής εκμάθησης](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>Από ένα πρόγραμμα-πελάτη ΥΠΌΛΟΙΠΟ

Υπηρεσίες Web του Azure μηχανικής εκμάθησης είναι RESTful APIs. Μπορείτε να εκμετάλλευση αυτά τα API από διάφορες πλατφόρμες, όπως .NET, Python, R, Java, κ.λπ. Η σελίδα **ανάλωση** της υπηρεσίας web στην [πύλη υπηρεσίες Web του Microsoft Azure μηχανικής εκμάθησης](https://services.azureml.net) έχει δείγμα κώδικα που μπορούν να σας βοηθήσουν να ξεκινήσετε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος για την εκμετάλλευση μιας υπηρεσίας web Azure μηχανικής εκμάθησης που έχει αναπτυχθεί από μια μηχανικής εκμάθησης έρευνας](machine-learning-consume-web-services.md).

