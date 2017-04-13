<properties 
    pageTitle="Παρακολούθηση και Διαχείριση ανάλυσης ροής εργασιών με το PowerShell | Microsoft Azure" 
    description="Μάθετε τον τρόπο χρήσης του Azure PowerShell και τα cmdlet για την παρακολούθηση και Διαχείριση ανάλυσης ροή εργασιών." 
    keywords="Azure powershell, cmdlet του azure powershell, εντολή powershell, δέσμες ενεργειών του powershell" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Παρακολούθηση και Διαχείριση ανάλυσης ροής εργασιών με το cmdlet του Azure PowerShell

Μάθετε πώς να παρακολουθείτε και να διαχειρίζεστε ροή ανάλυσης πόρων με cmdlet του Azure PowerShell και δέσμες ενεργειών του powershell που εκτελεί βασικές εργασίες ροής ανάλυσης.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Προϋποθέσεις για την εκτέλεση των cmdlet του Azure PowerShell για ανάλυση ροής

 - Δημιουργήστε μια ομάδα πόρων του Azure στη συνδρομή σας. Ακολουθεί ένα δείγμα δέσμης ενεργειών του PowerShell Azure. Για πληροφορίες Azure PowerShell, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md);  

Azure PowerShell 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Ροή εργασιών ανάλυσης δημιουργήθηκε μέσω προγραμματισμού δεν έχουν παρακολούθηση ενεργοποιημένη από προεπιλογή.  Μπορείτε να ενεργοποιήσετε με μη αυτόματο τρόπο παρακολούθησης στην πύλη του Azure, μεταβαίνοντας στη σελίδα παρακολούθηση του έργου και κάνοντας κλικ στο κουμπί Ενεργοποίηση ή μπορείτε να το κάνετε μέσω προγραμματισμού, ακολουθώντας τα βήματα που βρίσκεται στο [Azure ροή ανάλυση - οθόνη ροή ανάλυσης εργασιών ορθογώνιο](stream-analytics-monitor-jobs.md).

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure cmdlet του PowerShell για ανάλυση ροής
Τα παρακάτω cmdlet του Azure PowerShell μπορεί να χρησιμοποιηθεί για την παρακολούθηση και Διαχείριση ανάλυσης Azure ροή εργασιών. Σημειώστε ότι Azure PowerShell έχει διαφορετικές εκδόσεις. 
**Στα παραδείγματα που παρατίθενται είναι η πρώτη εντολή για το PowerShell Azure 0.9.8, η δεύτερη εντολή είναι για Azure PowerShell 1.0.** Οι εντολές Azure PowerShell 1.0 θα έχουν πάντα "AzureRM" στην εντολή.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Παραθέτει σε λίστα όλες τις εργασίες ροής ανάλυσης που ορίζονται από το στη Azure συνδρομής ή τη συγκεκριμένη ομάδα πόρων ή λαμβάνει πληροφορίες σχετικά με μια συγκεκριμένη εργασία μέσα σε μια ομάδα πόρων για την εργασία.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Αυτή η εντολή PowerShell επιστρέφει πληροφορίες σχετικά με όλες τις εργασίες ροής ανάλυσης στην Azure συνδρομής.

**Παράδειγμα 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Αυτή την εντολή PowerShell επιστρέφει πληροφορίες σχετικά με όλες τις εργασίες ροής ανάλυσης στην ομάδα πόρων StreamAnalytics προεπιλεγμένη-κεντρική-των Η.Π.Α.

**Παράδειγμα 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Αυτή την εντολή PowerShell επιστρέφει πληροφορίες σχετικά με την εργασία ροής ανάλυση StreamingJob στην ομάδα πόρων StreamAnalytics προεπιλεγμένη-κεντρική-των Η.Π.Α.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Παραθέτει σε λίστα όλα τα δεδομένα εισόδου που ορίζονται σε μια καθορισμένη εργασία ροής ανάλυση ή λαμβάνει πληροφορίες σχετικά με μια συγκεκριμένη εισαγωγής.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Αυτή η εντολή PowerShell επιστρέφει πληροφορίες σχετικά με όλα τα δεδομένα εισόδου που ορίζονται από το της εργασίας StreamingJob.

**Παράδειγμα 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Αυτή η εντολή PowerShell επιστρέφει πληροφορίες σχετικά με την είσοδο με το όνομα που ορίζονται από το της εργασίας StreamingJob EntryStream.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Παραθέτει σε λίστα όλων των προϊόντων που ορίζονται σε μια καθορισμένη εργασία ροής ανάλυση ή λαμβάνει πληροφορίες σχετικά με μια συγκεκριμένη εξόδου.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Αυτή η εντολή PowerShell επιστρέφει πληροφορίες για το εξόδους που ορίζονται από το της εργασίας StreamingJob.

**Παράδειγμα 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Αυτή η εντολή PowerShell επιστρέφει πληροφορίες για το αποτέλεσμα με το όνομα εξόδου που ορίζονται από το της εργασίας StreamingJob.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Λαμβάνει πληροφορίες σχετικά με το όριο των ροής μονάδες σε μια καθορισμένη περιοχή.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Αυτή η εντολή PowerShell επιστρέφει πληροφορίες σχετικά με το όριο και η χρήση της ροής μονάδες στην περιοχή κεντρικές ΗΠΑ.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Λαμβάνει πληροφορίες σχετικά με ένα συγκεκριμένο τύπο μετασχηματισμού που ορίζονται από το σε μια εργασία ροής ανάλυσης.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Αυτή η εντολή PowerShell επιστρέφει πληροφορίες για το μετασχηματισμό που ονομάζεται StreamingJob στην εργασία StreamingJob.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Νέα AzureStreamAnalyticsInput | Νέα AzureRMStreamAnalyticsInput
Δημιουργεί μια νέα εισόδου μέσα σε μια εργασία ροής ανάλυση ή ενημερώνεται ένα υπάρχον καθορισμένο εισαγωγής.
  
Το όνομα της εισόδου μπορεί να καθοριστεί στο αρχείο .json ή στη γραμμή εντολών. Εάν και τα δύο έχουν καθοριστεί, το όνομα της γραμμής εντολών πρέπει να είναι ίδια με αυτήν του αρχείου.

Εάν καθορίσετε μια εισαγωγή δεδομένων που υπάρχει ήδη και δεν καθορίσετε – ισχύ παραμέτρου, το cmdlet θα ερωτηθείτε αν θέλετε να αντικαταστήσετε το υπάρχον εισόδου.

Εάν καθορίσετε το – επιβάλετε παραμέτρου και καθορίστε μια υπάρχουσα εισαγάγετε όνομα, την είσοδο, θα αντικατασταθεί χωρίς επιβεβαίωση.

Για λεπτομερείς πληροφορίες σχετικά με τη δομή του αρχείου JSON και τα περιεχόμενα, ανατρέξτε στην [Δημιουργία εισαγωγής (ανάλυση ροή Azure)] [ msdn-rest-api-create-stream-analytics-input] ενότητα της [ροής ανάλυση διαχείρισης REST API αναφορά βιβλιοθήκη][stream.analytics.rest.api.reference].

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Αυτή η εντολή PowerShell δημιουργεί μια νέα εισαγωγή δεδομένων από το αρχείο Input.json. Εάν έχει οριστεί ήδη ένα υπάρχον εισαγωγής με το όνομα που καθορίζεται στο αρχείο ορισμού εισαγωγής, το cmdlet θα ερωτηθείτε αν θέλετε να αντικαταστήσετε.

**Παράδειγμα 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Αυτή η εντολή PowerShell δημιουργεί μια νέα εισόδου της εργασίας που ονομάζεται EntryStream. Εάν έχει οριστεί ήδη ένα υπάρχον εισαγωγής με αυτό το όνομα, το cmdlet θα ερωτηθείτε αν θέλετε να αντικαταστήσετε.

**Παράδειγμα 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Αυτή η εντολή PowerShell αντικαθιστά τον ορισμό των το υπάρχον αρχείο προέλευσης εισαγωγής που ονομάζεται EntryStream με τον ορισμό από το αρχείο.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Νέα AzureStreamAnalyticsJob | Νέα AzureRMStreamAnalyticsJob
Δημιουργεί μια νέα εργασία ροής ανάλυσης στο Microsoft Azure ή ενημερώνει τον ορισμό των μια υπάρχουσα εργασία καθορισμένο.

Το όνομα της εργασίας μπορεί να καθοριστεί στο αρχείο .json ή στη γραμμή εντολών. Εάν και τα δύο έχουν καθοριστεί, το όνομα της γραμμής εντολών πρέπει να είναι ίδια με αυτήν του αρχείου.

Εάν καθορίσετε το όνομα μιας εργασίας που υπάρχει ήδη και δεν καθορίσετε – ισχύ παραμέτρου, το cmdlet θα ερωτηθείτε αν θέλετε να αντικαταστήσετε το υπάρχον έργο.

Εάν καθορίσετε το – επιβάλετε παραμέτρου και καθορίστε ένα όνομα υπάρχοντος έργου, τον ορισμό εργασίας θα αντικατασταθούν χωρίς επιβεβαίωση.

Για λεπτομερείς πληροφορίες σχετικά με τη δομή του αρχείου JSON και τα περιεχόμενα, ανατρέξτε στην τη [Δημιουργία εργασίας ανάλυση ροή] [ msdn-rest-api-create-stream-analytics-job] ενότητα της [ροής ανάλυση διαχείρισης REST API αναφορά βιβλιοθήκη][stream.analytics.rest.api.reference].

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Αυτή η εντολή PowerShell δημιουργεί μια νέα εργασία από τον ορισμό στο JobDefinition.json. Εάν μια υπάρχουσα εργασία με το όνομα που καθορίζεται στο αρχείο ορισμού εργασίας έχει ήδη οριστεί, το cmdlet θα ερωτηθείτε αν θέλετε να αντικαταστήσετε.

**Παράδειγμα 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Αυτή η εντολή PowerShell αντικαθιστά τον ορισμό εργασίας για StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Νέα AzureStreamAnalyticsOutput | Νέα AzureRMStreamAnalyticsOutput
Δημιουργεί μια νέα εξόδου μέσα σε μια εργασία ροής ανάλυση ή ενημερώνεται ένα υπάρχον εξόδου.  

Το όνομα του αποτελέσματος μπορεί να καθοριστεί στο αρχείο .json ή στη γραμμή εντολών. Εάν και τα δύο έχουν καθοριστεί, το όνομα της γραμμής εντολών πρέπει να είναι ίδια με αυτήν του αρχείου.

Εάν καθορίσετε το αποτέλεσμα που υπάρχει ήδη και δεν καθορίσετε – ισχύ παραμέτρου, το cmdlet θα ερωτηθείτε αν θέλετε να αντικαταστήσετε το υπάρχον εξόδου.

Εάν καθορίσετε το – επιβάλετε παραμέτρου και καθορίστε μια υπάρχουσα εξόδου όνομα, το αποτέλεσμα θα αντικατασταθούν χωρίς επιβεβαίωση.

Για λεπτομερείς πληροφορίες σχετικά με τη δομή του αρχείου JSON και τα περιεχόμενα, ανατρέξτε στην [Δημιουργία εξόδου (ανάλυση ροή Azure)] [ msdn-rest-api-create-stream-analytics-output] ενότητα της [ροής ανάλυση διαχείρισης REST API αναφορά βιβλιοθήκη][stream.analytics.rest.api.reference].

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Αυτή η εντολή PowerShell δημιουργεί μια νέα εξόδου που ονομάζεται "Έξοδος" της εργασίας StreamingJob. Εάν έχει οριστεί ήδη υπάρχουσα το αποτέλεσμα με αυτό το όνομα, το cmdlet θα ερωτηθείτε αν θέλετε να αντικαταστήσετε.

**Παράδειγμα 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Αυτή η εντολή PowerShell αντικαθιστά τον ορισμό για "Έξοδος" της εργασίας StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Νέα AzureStreamAnalyticsTransformation | Νέα AzureRMStreamAnalyticsTransformation
Δημιουργεί μια νέα μετασχηματισμό μέσα σε μια εργασία ροής ανάλυση ή ενημερώνει το υπάρχον μετασχηματισμό.
  
Το όνομα του μετασχηματισμού μπορεί να καθοριστεί στο αρχείο .json ή στη γραμμή εντολών. Εάν και τα δύο έχουν καθοριστεί, το όνομα της γραμμής εντολών πρέπει να είναι ίδια με αυτήν του αρχείου.

Εάν καθορίσετε έναν μετασχηματισμό που υπάρχει ήδη και δεν καθορίσετε – ισχύ παραμέτρου, το cmdlet θα ερωτηθείτε αν θέλετε να αντικαταστήσετε το υπάρχον μετασχηματισμό.

Εάν καθορίσετε το – επιβάλετε παραμέτρου και καθορίστε ένα όνομα υπάρχοντος μετασχηματισμό, το μετασχηματισμό θα αντικατασταθούν χωρίς επιβεβαίωση.

Για λεπτομερείς πληροφορίες σχετικά με τη δομή του αρχείου JSON και τα περιεχόμενα, ανατρέξτε στην το [Μετασχηματισμό Δημιουργία (ανάλυση ροή Azure)] [ msdn-rest-api-create-stream-analytics-transformation] ενότητα της [ροής ανάλυση διαχείρισης REST API αναφορά βιβλιοθήκη][stream.analytics.rest.api.reference].

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Αυτή η εντολή PowerShell δημιουργεί μια νέα μετασχηματισμό που ονομάζεται StreamingJobTransform στην εργασία StreamingJob. Εάν μια υπάρχουσα μετασχηματισμό έχει ήδη οριστεί με αυτό το όνομα, το cmdlet θα ερωτηθείτε αν θέλετε να αντικαταστήσετε.

**Παράδειγμα 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Αυτή η εντολή PowerShell αντικαθιστά τον ορισμό των StreamingJobTransform της εργασίας StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Κατάργηση AzureStreamAnalyticsInput | Κατάργηση AzureRMStreamAnalyticsInput
Ασύγχρονη διαγράφει ένα συγκεκριμένο εισαγωγή δεδομένων από μια εργασία ροής ανάλυσης στο Microsoft Azure.  
Εάν καθορίσετε – ισχύ παραμέτρου, τα δεδομένα εισόδου θα διαγραφεί χωρίς επιβεβαίωση.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Αυτή η εντολή PowerShell καταργεί το εισαγωγής EventStream της εργασίας StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Κατάργηση AzureStreamAnalyticsJob | Κατάργηση AzureRMStreamAnalyticsJob
Ασύγχρονη διαγράφει μια συγκεκριμένη εργασία ροή ανάλυσης στο Microsoft Azure.  
Εάν καθορίσετε – ισχύ παραμέτρου, θα διαγραφεί η εργασία χωρίς επιβεβαίωση.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Αυτή η εντολή PowerShell καταργεί την εργασία StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Κατάργηση AzureStreamAnalyticsOutput | Κατάργηση AzureRMStreamAnalyticsOutput
Ασύγχρονη διαγράφει ένα συγκεκριμένο εξόδου από μια εργασία ροής ανάλυσης στο Microsoft Azure.  
Εάν καθορίσετε – ισχύ παραμέτρου, το αποτέλεσμα θα διαγραφεί χωρίς επιβεβαίωση.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Σε αυτό το αποτέλεσμα εξόδου στη εργασία StreamingJob καταργεί εντολή PowerShell.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Έναρξη AzureStreamAnalyticsJob | Έναρξη-AzureRMStreamAnalyticsJob
Ασύγχρονη αναπτύσσεται και ξεκινά μια εργασία ροής ανάλυσης στο Microsoft Azure.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Αυτή η εντολή PowerShell ξεκινά η εργασία StreamingJob με ώρα έναρξης εξόδου προσαρμοσμένη ρύθμιση 12 Δεκεμβρίου 2012, 12:12:12 UTC.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Διακοπή AzureStreamAnalyticsJob | Διακοπή AzureRMStreamAnalyticsJob
Ασύγχρονη σταματά μιας εργασίας ροής ανάλυσης από εκτελείται στο Microsoft Azure και καταργήστε την εκχωρούν πόρους που έχουν που έχουν που χρησιμοποιείται. Τον ορισμό εργασίας και μετα-δεδομένων θα παραμείνουν διαθέσιμοι εντός τη συνδρομή σας μέσω τόσο την πύλη του Azure και τη Διαχείριση API, τέτοια ώστε η εργασία μπορεί να υποστεί επεξεργασία και επανεκκίνηση του. Δεν θα χρεωθεί για μια εργασία σε κατάσταση διακοπής.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Αυτή η εντολή PowerShell διακόπτει την εργασία StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Δοκιμή AzureStreamAnalyticsInput | Δοκιμή AzureRMStreamAnalyticsInput
Ελέγχει τη δυνατότητα της ανάλυσης ροής για να συνδεθείτε με μια καθορισμένη εισαγωγής.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Αυτή η εντολή PowerShell ελέγχει την κατάσταση σύνδεσης του το εισαγωγής EntryStream στο StreamingJob.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Δοκιμή AzureStreamAnalyticsOutput | Δοκιμή AzureRMStreamAnalyticsOutput
Ελέγχει τη δυνατότητα της ανάλυσης ροής για να συνδεθείτε με μια καθορισμένη εξόδου.

**Παράδειγμα 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Αυτήν την κατάσταση σύνδεσης του αποτελέσματος εξόδου στο StreamingJob δοκιμές εντολή PowerShell.  

## <a name="get-support"></a>Λήψη υποστήριξης
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ Azure ροή ανάλυσης](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 


## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
