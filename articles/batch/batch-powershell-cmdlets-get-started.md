<properties
   pageTitle="Γρήγορα αποτελέσματα με το PowerShell δέσμη Azure | Microsoft Azure"
   description="Μια γρήγορη εισαγωγή για να τα cmdlet του PowerShell Azure μπορείτε να χρησιμοποιήσετε για να διαχειριστείτε την υπηρεσία δέσμη Azure"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Γρήγορα αποτελέσματα με το cmdlet του PowerShell δέσμη Azure
Με τα cmdlet του PowerShell δέσμη Azure, μπορείτε να πραγματοποιήσετε και δέσμη ενεργειών πολλές από τις ίδιες εργασίες που μπορείτε να εκτελέσετε με τα API δέσμη, την πύλη του Azure και το περιβάλλον γραμμής εντολών Azure (CLI). Αυτή είναι μια σύντομη εισαγωγή τα cmdlet που μπορείτε να χρησιμοποιήσετε για να διαχειριστείτε τους λογαριασμούς σας δέσμης και να εργαστείτε με τους πόρους σας δέσμη όπως σύνολα, εργασίες και εργασίες.

Για μια πλήρη λίστα των cmdlet δέσμης και σύνταξη λεπτομερείς cmdlet, ανατρέξτε στο άρθρο [αναφορά cmdlet δέσμη Azure](https://msdn.microsoft.com/library/azure/mt125957.aspx).

Σε αυτό το άρθρο βασίζεται σε cmdlet στο Azure PowerShell έκδοση 3.0.0. Συνιστάται να ενημερώσετε το Azure PowerShell συχνά για να επωφεληθείτε από την υπηρεσία ενημερωμένες εκδόσεις και βελτιώσεις.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Εκτελέστε τις ακόλουθες ενέργειες για να χρησιμοποιήσετε Azure PowerShell για να διαχειριστείτε τους πόρους σας δέσμης.

* [Εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md)

* Εκτελέστε το cmdlet **AzureRmAccount σύνδεσης** για να συνδεθείτε με τη συνδρομή σας (το Azure δέσμη cmdlet για αποστολή στη λειτουργική μονάδα Azure από διαχειριστή πόρων):

    `Login-AzureRmAccount`

* **Καταχώρηση με το χώρο ονομάτων παροχής δέσμης**. Αυτή η λειτουργία μόνο πρέπει να έχει πραγματοποιηθεί **μία φορά για κάθε εγγραφή**.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Διαχείριση λογαριασμών δέσμης και κλειδιά

### <a name="create-a-batch-account"></a>Δημιουργία λογαριασμού δέσμης

**Δημιουργία AzureRmBatchAccount** δημιουργεί ένα λογαριασμό δέσμη σε μια συγκεκριμένη ομάδα πόρων. Εάν δεν έχετε ήδη μια ομάδα πόρων, δημιουργήστε μία, εκτελώντας το cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) . Καθορίστε μία από τις περιοχές Azure στην παράμετρο **θέση** , όπως η "Κεντρική η.π.α.". Για παράδειγμα:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Στη συνέχεια, δημιουργήστε ένα λογαριασμό δέσμη στην ομάδα πόρων, καθορίζοντας ένα όνομα για το λογαριασμό στο <*όνομα_λογαριασμού*> και τη θέση και το όνομα της ομάδας πόρων. Δημιουργία του λογαριασμού δέσμη μπορεί να χρειαστεί κάποιος χρόνος για να ολοκληρωθεί. Για παράδειγμα:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] Το όνομα του λογαριασμού δέσμη πρέπει να είναι μοναδική στην περιοχή Azure για την ομάδα πόρων περιέχουν μεταξύ 3 και 24 χαρακτήρες και χρήση μόνο πεζά γράμματα και αριθμούς.

### <a name="get-account-access-keys"></a>Λάβετε τα πλήκτρα πρόσβασης λογαριασμού
**Get-AzureRmBatchAccountKeys** εμφανίζει τα πλήκτρα πρόσβασης που σχετίζεται με ένα λογαριασμό δέσμη Azure. Για παράδειγμα, εκτελέστε τα εξής για να λάβετε τα πλήκτρα κύριας και δευτερεύουσας του λογαριασμού που δημιουργήσατε.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Δημιουργία ενός νέου κλειδιού πρόσβασης
**Δημιουργία AzureRmBatchAccountKey** δημιουργεί ένα νέο λογαριασμό πρωτεύον ή δευτερεύον κλειδί για ένα λογαριασμό δέσμη Azure. Για παράδειγμα, για να δημιουργήσετε ένα νέο πρωτεύον κλειδί για το λογαριασμό σας δέσμης, πληκτρολογήστε:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Για να δημιουργήσετε ένα νέο δευτερεύον κλειδί, καθορίστε "Δευτερεύουσα" για την παράμετρο **τύπο κλειδιού** . Πρέπει να αναδημιουργήσετε τα πλήκτρα κύριας και δευτερεύουσας ξεχωριστά.

### <a name="delete-a-batch-account"></a>Διαγραφή ενός λογαριασμού δέσμης
**Κατάργηση AzureRmBatchAccount** διαγράφει ένα λογαριασμό δέσμης. Για παράδειγμα:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Όταν σας ζητηθεί, επιβεβαιώστε ότι θέλετε να καταργήσετε το λογαριασμό. Κατάργηση λογαριασμού μπορεί να χρειαστεί κάποιος χρόνος για να ολοκληρωθεί.

## <a name="create-a-batchaccountcontext-object"></a>Δημιουργία ενός αντικειμένου BatchAccountContext

Για τον έλεγχο ταυτότητας με τα cmdlet του PowerShell δέσμη όταν δημιουργείτε και διαχείριση χώρους συγκέντρωσης δέσμη, εργασίες, εργασίες και άλλοι πόροι, πρέπει πρώτα να δημιουργήσετε ένα αντικείμενο BatchAccountContext για να αποθηκεύσετε το όνομα λογαριασμού και πλήκτρα:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Μπορείτε να μεταβιβάσετε το αντικείμενο BatchAccountContext σε cmdlet που θα χρησιμοποιήσετε την παράμετρο **BatchContext** .

> [AZURE.NOTE] Από προεπιλογή, το λογαριασμό πρωτεύον κλειδί χρησιμοποιείται για τον έλεγχο ταυτότητας, αλλά μπορείτε να επιλέξετε ρητά το κλειδί για να χρησιμοποιήσετε, αλλάζοντας την ιδιότητα **KeyInUse** του αντικειμένου σας BatchAccountContext: `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Δημιουργία και τροποποίηση δέσμη πόροι
Χρήση των cmdlet όπως **Δημιουργία AzureBatchPool**, **Δημιουργία AzureBatchJob**και **Δημιουργία AzureBatchTask** για τη δημιουργία πόρους στην περιοχή ο λογαριασμός μια δέσμη. Είναι που αντιστοιχούν **Get -** και cmdlet **Set-** για να ενημερώσετε τις ιδιότητες των υπαρχόντων πόρων και **Κατάργηση-** cmdlet για να καταργήσετε πόρους στην περιοχή ο λογαριασμός μια δέσμη.

Όταν χρησιμοποιείτε πολλές από αυτών των cmdlet, εκτός από που περνά μέσα σε ένα αντικείμενο BatchContext, πρέπει να δημιουργήσετε ή να μεταβιβάζουν αντικείμενα που περιέχουν πόρων λεπτομερείς ρυθμίσεις, όπως φαίνεται στο παρακάτω παράδειγμα. Δείτε τη λεπτομερή βοήθεια για κάθε cmdlet για περισσότερα παραδείγματα.

### <a name="create-a-batch-pool"></a>Δημιουργία χώρου συγκέντρωσης δέσμης

Κατά τη δημιουργία ή ενημέρωση χώρου συγκέντρωσης δέσμη, μπορείτε να επιλέξετε μια ρύθμιση παραμέτρων της υπηρεσίας cloud ή μια εικονική μηχανή ρύθμιση παραμέτρων για το λειτουργικό σύστημα σε κόμβους υπολογιστικών (ανατρέξτε στο θέμα [επισκόπηση δυνατοτήτων δέσμης](batch-api-basics.md#pool)). Η επιλογή καθορίζει αν είναι απεικόνιση σας κόμβους υπολογιστικών με ένα από τα [εκδόσεις OS επισκέπτη Azure](../cloud-services/cloud-services-guestos-update-matrix.md#releases) ή με μία από τις υποστηριζόμενες Linux ή Windows Εικονική εικόνες από το Azure Marketplace.

Όταν εκτελείτε το **Νέο AzureBatchPool**, μεταβιβάζουν τις ρυθμίσεις του λειτουργικού συστήματος σε ένα αντικείμενο PSCloudServiceConfiguration ή PSVirtualMachineConfiguration. Για παράδειγμα, το ακόλουθο cmdlet δημιουργεί ένα νέο σύνολο δέσμη με κόμβους υπολογιστικών μικρό μέγεθος στις παραμέτρους της υπηρεσίας cloud, με απεικόνιση με την πιο πρόσφατη έκδοση του λειτουργικού συστήματος της οικογένειας 3 (Windows Server 2012). Εδώ, η παράμετρος **CloudServiceConfiguration** Καθορίζει τη μεταβλητή *$configuration* ως το αντικείμενο PSCloudServiceConfiguration. Η παράμετρος **BatchContext** καθορίζει μια παλιότερα καθορισμένη μεταβλητής *$context* ως το αντικείμενο BatchAccountContext.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Ο αριθμός προορισμού κόμβους υπολογιστικών στο νέο σύνολο καθορίζεται από έναν τύπο autoscaling. Σε αυτήν την περίπτωση, ο τύπος είναι απλώς **$TargetDedicated = 4**, που υποδεικνύει τον αριθμό των κόμβους υπολογιστικών στο χώρο συγκέντρωσης είναι πολύ 4.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Ερώτημα για χώρους συγκέντρωσης, εργασίες, εργασίες και άλλες λεπτομέρειες

Χρησιμοποιήστε τα cmdlet για όπως **Get-AzureBatchPool**, **Get-AzureBatchJob**και **Get-AzureBatchTask** ερώτημα για οντοτήτων δημιουργηθεί κάτω από ένα λογαριασμό δέσμης.

### <a name="query-for-data"></a>Ερωτήματος για δεδομένα

Ως παράδειγμα, χρησιμοποιήστε **Get-AzureBatchPools** για να βρείτε το χώρους συγκέντρωσης. Από προεπιλογή αυτή ερωτήματα για όλους τους χώρους συγκέντρωσης κάτω από το λογαριασμό σας, υπό την προϋπόθεση ότι έχετε ήδη αποθηκευμένο το αντικείμενο BatchAccountContext στο *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Χρησιμοποιήστε ένα φίλτρο OData

Μπορείτε να δώσετε ένα φίλτρο OData χρησιμοποιώντας την παράμετρο **φίλτρου** για να βρείτε μόνο τα αντικείμενα που σας ενδιαφέρει. Για παράδειγμα, μπορείτε να βρείτε όλους τους χώρους συγκέντρωσης με αναγνωριστικά που ξεκινούν με "myPool":

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Η μέθοδος αυτή δεν είναι ως ευέλικτη ως χρήση "Θέση αντικειμένου" σε μια τοπική διοχέτευσης. Ωστόσο, το ερώτημα αποστέλλονται στην υπηρεσία δέσμη απευθείας, έτσι ώστε όλα τα φίλτρα, γίνεται από την πλευρά του διακομιστή, αποθήκευση εύρος ζώνης Internet.

### <a name="use-the-id-parameter"></a>Χρησιμοποιήσετε την παράμετρο Id

Εναλλακτική λύση για ένα φίλτρο OData είναι να χρησιμοποιήσετε την παράμετρο **αναγνωριστικό** . Για να υποβάλετε ερώτημα για ένα συγκεκριμένο σύνολο με το αναγνωριστικό "myPool":

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Η παράμετρος **αναγνωριστικού** υποστηρίζει μόνο πλήρους αναγνωριστικού αναζήτησης, δεν χαρακτήρες μπαλαντέρ ή φίλτρα OData στυλ.

### <a name="use-the-maxcount-parameter"></a>Χρησιμοποιήστε την παράμετρο MaxCount

Από προεπιλογή, κάθε cmdlet επιστρέφει έως 1000 αντικειμένων. Εάν φτάσετε αυτό το όριο, είτε να περιορίσετε περισσότερο το φίλτρο για να εμφανίσετε ξανά λιγότερα αντικείμενα ή έχουν οριστεί αποκλειστικά έως χρησιμοποιώντας την παράμετρο **MaxCount** . Για παράδειγμα:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Για να καταργήσετε το ανώτερο όριο, ορίστε **MaxCount** σε 0 ή λιγότερο.

### <a name="use-the-powershell-pipeline"></a>Χρήση της διοχέτευσης PowerShell

Μαζική cmdlet για να αξιοποιήσετε τη διαδικασία PowerShell για την αποστολή δεδομένων μεταξύ των cmdlet. Αυτό έχει το ίδιο αποτέλεσμα με τον καθορισμό μιας παραμέτρου, αλλά διευκολύνει την εργασία με πολλά οντοτήτων.

Για παράδειγμα, βρείτε και να εμφανίσετε όλες τις εργασίες στην περιοχή ο λογαριασμός σας:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Επανεκκίνηση (επανεκκίνηση) κάθε κόμβος σε ένα χώρο συγκέντρωσης:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Διαχείριση πακέτου εφαρμογών

Πακέτων εφαρμογών παρέχει απλοποιημένη τρόπο για την ανάπτυξη εφαρμογών για τους κόμβους υπολογισμού σας χώρους συγκέντρωσης. Με τα cmdlet του PowerShell δέσμη, μπορείτε να αποστείλετε και διαχείριση πακέτων εφαρμογών στο λογαριασμό σας δέσμης και ανάπτυξη του πακέτου εκδόσεις για τον υπολογισμό κόμβους.

**Δημιουργία** μιας εφαρμογής:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Προσθήκη** ενός πακέτου εφαρμογής:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Ορίστε την **προεπιλεγμένη έκδοση** για την εφαρμογή:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Λίστα** πακέτων μιας εφαρμογής

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Διαγράψτε** ένα πακέτο εφαρμογών

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Διαγραφή** μιας εφαρμογής

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] Πρέπει να διαγράψετε όλων των εκδόσεων του πακέτου εφαρμογής μιας εφαρμογής πριν να διαγράψετε την εφαρμογή. Θα λάβετε ένα σφάλμα 'Διένεξης' Εάν προσπαθείτε να διαγράψετε μια εφαρμογή η οποία περιλαμβάνει τη συγκεκριμένη στιγμή πακέτων εφαρμογών.

### <a name="deploy-an-application-package"></a>Ανάπτυξη ένα πακέτο εφαρμογών

Μπορείτε να καθορίσετε μία ή περισσότερες πακέτων εφαρμογών για ανάπτυξη κατά τη δημιουργία ενός χώρου συγκέντρωσης. Όταν ορίζετε ένα πακέτο κατά τη δημιουργία χώρου συγκέντρωσης, το έχει αναπτυχθεί σε κάθε κόμβο με το χώρο συγκέντρωσης σύνδεσμοι κόμβο. Πακέτα αναπτύσσονται επίσης όταν έχει γίνει επανεκκίνηση ή reimaged έναν κόμβο.

Καθορίστε το `-ApplicationPackageReference` την επιλογή κατά τη δημιουργία ενός χώρου συγκέντρωσης για να υλοποιήσετε ένα πακέτο εφαρμογών για το χώρο συγκέντρωσης κόμβους συνδέονται το χώρο συγκέντρωσης. Πρώτα, δημιουργήστε ένα αντικείμενο **PSApplicationPackageReference** και ρυθμίστε τις παραμέτρους του με το αναγνωριστικό και πακέτο έκδοση εφαρμογής που θέλετε να αναπτύξετε για το χώρο συγκέντρωσης κόμβους υπολογιστικών:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Τώρα, δημιουργήστε το χώρο συγκέντρωσης και καθορίστε το αντικείμενο αναφοράς πακέτου ως το όρισμα το `ApplicationPackageReferences` επιλογή:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με πακέτων εφαρμογών στην [ανάπτυξη εφαρμογών με το Azure δέσμη πακέτων εφαρμογών](batch-application-packages.md).

>[AZURE.IMPORTANT] Πρέπει να [σύνδεση αποθήκευσης Azure λογαριασμού](#linked-storage-account-autostorage) στο λογαριασμό σας δέσμη για να χρησιμοποιήσετε πακέτων εφαρμογών.

### <a name="update-a-pools-application-packages"></a>Ενημέρωση χώρου συγκέντρωσης πακέτων εφαρμογών

Για να ενημερώσετε τις εφαρμογές που έχουν ανατεθεί σε έναν υπάρχοντα χώρο συγκέντρωσης, δημιουργήστε πρώτα ένα αντικείμενο PSApplicationPackageReference με τις επιθυμητές ιδιότητες (αναγνωριστικό και πακέτο έκδοση εφαρμογής):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Στη συνέχεια, λάβετε το χώρο συγκέντρωσης από δέσμη, εκκαθαρίσετε οποιαδήποτε υπάρχοντα πακέτα, προσθέστε τη νέα αναφορά πακέτου και ενημέρωση της υπηρεσίας δέσμη με τις νέες ρυθμίσεις χώρου συγκέντρωσης:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Τώρα έχετε ενημερώσει το χώρο συγκέντρωσης ιδιότητες στην υπηρεσία δέσμης. Για να αναπτύξετε στην πραγματικότητα το νέο πακέτο εφαρμογών για τον υπολογισμό κόμβους στο χώρο συγκέντρωσης, ωστόσο, πρέπει να κάνετε επανεκκίνηση ή reimage κόμβους. Μπορείτε να κάνετε επανεκκίνηση κάθε κόμβο σε ένα χώρο συγκέντρωσης με αυτήν την εντολή:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] Μπορείτε να αναπτύξετε πολλών πακέτων εφαρμογών σε κόμβους υπολογιστικών σε ένα χώρο συγκέντρωσης. Εάν θέλετε να *προσθέσετε* ένα πακέτο εφαρμογών αντί για αντικαθιστώντας τα πακέτα ανεπτυγμένος αυτήν τη στιγμή, παραλείψτε την `$pool.ApplicationPackageReferences.Clear()` παραπάνω γραμμή.

## <a name="next-steps"></a>Επόμενα βήματα

* Για λεπτομερείς cmdlet σύνταξη και παραδείγματα, ανατρέξτε στο θέμα [αναφορά cmdlet δέσμη Azure](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Για περισσότερες πληροφορίες σχετικά με τις εφαρμογές και πακέτων εφαρμογών στη δέσμη, ανατρέξτε στο θέμα [ανάπτυξη εφαρμογών με το Azure δέσμη πακέτων εφαρμογών](batch-application-packages.md).
