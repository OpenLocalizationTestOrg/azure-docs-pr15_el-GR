<properties
    pageTitle="Azure PowerShell για τη διαχείριση πόρων βάσει εντολές για την εφαρμογή Web Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε τις νέες εντολές Azure από διαχειριστή πόρων βάσει του PowerShell για τη Διαχείριση εφαρμογών Web της Azure."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Χρήση του PowerShell βάσει Manager Azure πόρων για τη Διαχείριση εφαρμογών Azure Web#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Με το Microsoft Azure PowerShell έκδοση 1.0.0 νέων εντολών έχουν προστεθεί, που δίνει στο χρήστη τη δυνατότητα να χρησιμοποιήσετε τις εντολές του PowerShell που βασίζονται στο Azure από διαχειριστή πόρων για τη Διαχείριση εφαρμογών Web.

Για να μάθετε σχετικά με τη διαχείριση ομάδων πόρων, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](../powershell-azure-resource-manager.md). 

Για να μάθετε περισσότερα σχετικά με την πλήρη λίστα των παραμέτρων και επιλογές για τα cmdlet του PowerShell, ανατρέξτε στην [Πλήρη αναφορές για τα Cmdlet του διαχειριστή πόρων Azure εφαρμογών Web βάσει των cmdlet του PowerShell](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Διαχείριση εφαρμογών υπηρεσίας προγράμματος ##

### <a name="create-an-app-service-plan"></a>Δημιουργήστε ένα πρόγραμμα εφαρμογής υπηρεσίας ###
Για να δημιουργήσετε ένα σχέδιο παροχής υπηρεσιών εφαρμογής, χρησιμοποιήστε το cmdlet **New-AzureRmAppServicePlan** .

Ακολουθούν περιγραφές από τις διαφορετικές παραμέτρους:

-   **Όνομα**: όνομα του σχέδιο παροχής υπηρεσιών εφαρμογής.
-   **Θέση**: υπηρεσίας πρόγραμμα θέση.
-   **ResourceGroupName**: ομάδα πόρων που περιλαμβάνει το πρόγραμμα υπηρεσιών που έχουν δημιουργηθεί πρόσφατα εφαρμογής.
-   **Επίπεδο**: την επιθυμητή σειρά τιμολόγησης (η προεπιλογή είναι δωρεάν, οι άλλες επιλογές είναι κοινόχρηστα, Basic, τυπική και Premium.)
-   **WorkerSize**: το μέγεθος των εργαζομένων (η προεπιλογή είναι μικρές εάν η παράμετρος επίπεδο έχει καθοριστεί ως Basic, τυπική ή Premium. Άλλες επιλογές είναι Μεσαίο και μεγάλο.)
-   **NumberofWorkers**: ο αριθμός των εργαζομένων στην εφαρμογή υπηρεσίας σχέδιο (η προεπιλεγμένη τιμή είναι 1). 

Παράδειγμα για να χρησιμοποιήσετε αυτό το cmdlet:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Δημιουργία μιας εφαρμογής υπηρεσίας σχεδίου σε ένα περιβάλλον εφαρμογής υπηρεσίας ###
Για να δημιουργήσετε ένα σχέδιο παροχής υπηρεσιών εφαρμογής σε ένα περιβάλλον υπηρεσίας εφαρμογής, χρησιμοποιήστε την ίδια εντολή **Δημιουργία AzureRmAppServicePlan** εντολή με επιπλέον παραμέτρους για να καθορίσετε το όνομα του ASE και του ASE όνομα της ομάδας πόρων.

Παράδειγμα για να χρησιμοποιήσετε αυτό το cmdlet:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Για να μάθετε περισσότερα σχετικά με την εφαρμογή υπηρεσίας περιβάλλοντος, επιλέξτε [Εισαγωγή στην εφαρμογή υπηρεσίας περιβάλλον](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Λίστα υπάρχοντα σχέδια εφαρμογής υπηρεσίας ###

Για να παραθέσετε τα υπάρχοντα σχέδια εφαρμογής υπηρεσίας, χρησιμοποιήστε το cmdlet **Get-AzureRmAppServicePlan** .

Για να δημιουργήσετε μια λίστα με όλα τα προγράμματα υπηρεσίας εφαρμογής κάτω από τη συνδρομή σας, χρησιμοποιήστε: 

    Get-AzureRmAppServicePlan

Για να δημιουργήσετε μια λίστα με όλα τα προγράμματα υπηρεσίας εφαρμογής στην περιοχή μιας συγκεκριμένης ομάδας πόρων, χρησιμοποιήστε:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Για να λάβετε μια συγκεκριμένη εφαρμογή σχέδιο παροχής υπηρεσιών, χρησιμοποιήστε:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Ρύθμιση παραμέτρων ενός υπάρχοντος σχεδίου υπηρεσίας εφαρμογής ###

Για να αλλάξετε τις ρυθμίσεις για ένα υπάρχον σχέδιο της εφαρμογής υπηρεσίας, χρησιμοποιήστε το cmdlet **Set-AzureRmAppServicePlan** . Μπορείτε να αλλάξετε τη σειρά, το μέγεθος της εργασίας και τον αριθμό των εργαζομένων 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Κλιμάκωση ένα πρόγραμμα εφαρμογής υπηρεσίας ####

Για να περιορίσετε το μέγεθος ενός υπάρχοντος σχεδίου υπηρεσίας εφαρμογής, χρησιμοποιήστε:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Αλλαγή του μεγέθους εργαζόμενου ενός σχεδίου εφαρμογών υπηρεσίας ####

Για να αλλάξετε το μέγεθος των εργαζομένων σε ένα υπάρχον πρόγραμμα υπηρεσίας εφαρμογής, χρησιμοποιήστε:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Αλλαγή σειράς από ένα πρόγραμμα εφαρμογής υπηρεσίας ####

Για να αλλάξετε τη σειρά από ένα υπάρχον πρόγραμμα υπηρεσίας εφαρμογής, χρησιμοποιήστε:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Διαγραφή ενός υπάρχοντος σχεδίου υπηρεσίας εφαρμογής ###

Για να διαγράψετε ένα υπάρχον σχέδιο της εφαρμογής υπηρεσίας, όλες οι εφαρμογές web που του έχουν ανατεθεί πρέπει να είναι μετακινήθηκε ή διαγράφηκε πρώτα. Στη συνέχεια, χρησιμοποιώντας το cmdlet **Κατάργηση AzureRmAppServicePlan** μπορείτε να διαγράψετε το σχέδιο παροχής υπηρεσιών εφαρμογής.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Διαχείριση εφαρμογών Web εφαρμογής υπηρεσίας ##

### <a name="create-a-web-app"></a>Δημιουργία εφαρμογής Web ###

Για να δημιουργήσετε μια εφαρμογή web, χρησιμοποιήστε το cmdlet **New-AzureRmWebApp** .

Ακολουθούν περιγραφές από τις διαφορετικές παραμέτρους:

- **Όνομα**: όνομα για την εφαρμογή web.
- **AppServicePlan**: όνομα για το πρόγραμμα υπηρεσιών που χρησιμοποιείται για τη φιλοξενία της εφαρμογής web.
- **ResourceGroupName**: ομάδα πόρων που φιλοξενεί το πρόγραμμα υπηρεσιών εφαρμογής.
- **Θέση**: τη θέση της εφαρμογής web.

Παράδειγμα για να χρησιμοποιήσετε αυτό το cmdlet:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Δημιουργία εφαρμογής Web σε ένα περιβάλλον εφαρμογής υπηρεσίας ###

Για να δημιουργήσετε μια εφαρμογή web σε μια εφαρμογή υπηρεσίας περιβάλλον (ASE). Χρησιμοποιήστε την ίδια εντολή **Δημιουργία AzureRmWebApp** με επιπλέον παραμέτρους για να καθορίσετε το όνομα ASE και το όνομα της ομάδας πόρων στην οποία ανήκει το ASE.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Για να μάθετε περισσότερα σχετικά με την εφαρμογή υπηρεσίας περιβάλλοντος, επιλέξτε [Εισαγωγή στην εφαρμογή υπηρεσίας περιβάλλον](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Διαγράψτε μια υπάρχουσα εφαρμογή Web ###

Για να διαγράψετε μια υπάρχουσα εφαρμογή web, μπορείτε να χρησιμοποιήσετε το cmdlet **Κατάργηση AzureRmWebApp** , πρέπει να καθορίσετε το όνομα του web app και το όνομα της ομάδας πόρων.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Εφαρμογές Web της υπάρχουσας λίστας ###

Για να δημιουργήσετε μια λίστα με τις υπάρχουσες εφαρμογές web, χρησιμοποιήστε το cmdlet **Get-AzureRmWebApp** .

Για να δημιουργήσετε μια λίστα με όλες τις εφαρμογές web κάτω από τη συνδρομή σας, χρησιμοποιήστε:

    Get-AzureRmWebApp

Για να δημιουργήσετε μια λίστα με όλες τις εφαρμογές web στην περιοχή μιας συγκεκριμένης ομάδας πόρων, χρησιμοποιήστε:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Για να λάβετε μια εφαρμογή web συγκεκριμένες, χρησιμοποιήστε:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Ρύθμιση παραμέτρων σε μια υπάρχουσα εφαρμογή Web ###

Για να αλλάξετε τις ρυθμίσεις και ρυθμίσεις παραμέτρων για μια υπάρχουσα εφαρμογή web, χρησιμοποιήστε το cmdlet **Set-AzureRmWebApp** . Για μια πλήρη λίστα των παραμέτρων, επιλέξτε τη [σύνδεση αναφοράς Cmdlet](https://msdn.microsoft.com/library/mt652487.aspx)

Παράδειγμα (1): Χρησιμοποιήστε αυτό το cmdlet για να αλλάξετε συμβολοσειρές σύνδεσης

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Παράδειγμα (2): Προσθήκη ή αλλαγή των ρυθμίσεων της εφαρμογής

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Παράδειγμα (3): Ορισμός της εφαρμογής web να εκτελείται σε λειτουργία 64-bit

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Να αλλάξετε την κατάσταση από μια υπάρχουσα εφαρμογή Web ###

#### <a name="restart-a-web-app"></a>Επανεκκινήστε μια εφαρμογή web ####

Για να ξεκινήσετε ξανά μια εφαρμογή web, πρέπει να καθορίσετε την ομάδα όνομα και πόρων της εφαρμογής web.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Διακοπή μια εφαρμογή web ####

Για να διακόψετε μια εφαρμογή web, πρέπει να καθορίσετε την ομάδα όνομα και πόρων της εφαρμογής web.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Ξεκινήστε μια εφαρμογή web ####

Για να ξεκινήσετε μια εφαρμογή web, πρέπει να καθορίσετε την ομάδα όνομα και πόρων της εφαρμογής web.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Διαχείριση προφίλ εφαρμογή δημοσίευσης στο Web ###

Κάθε εφαρμογή web έχει ένα προφίλ δημοσίευσης που μπορούν να χρησιμοποιηθούν για να δημοσιεύσετε τις εφαρμογές σας, μπορεί να εκτελεστεί περισσότερες από μία εργασίες για τη δημοσίευση προφίλ.

#### <a name="get-publishing-profile"></a>Λήψη δημοσίευσης προφίλ ####

Για να λάβετε το προφίλ δημοσίευσης για μια εφαρμογή web, χρησιμοποιήστε:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Αυτή η εντολή αντηχήσεων το προφίλ δημοσίευσης στη γραμμή εντολών ως επίσης και εξόδου στο προφίλ δημοσίευσης σε ένα αρχείο κειμένου.

#### <a name="reset-publishing-profile"></a>Επαναφορά δημοσίευσης προφίλ ####

Για να επαναφέρετε και τα δύο δημοσίευσης κωδικού πρόσβασης για το web και FTP αναπτύξετε για μια εφαρμογή web, χρησιμοποιήστε:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Διαχείριση πιστοποιητικών εφαρμογής Web ###

Για να μάθετε σχετικά με τη διαχείριση πιστοποιητικών εφαρμογής web, ανατρέξτε στο θέμα [χρήση του PowerShell σύνδεση πιστοποιητικά SSL](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Επόμενα βήματα ###
- Για να μάθετε σχετικά με την υποστήριξη του PowerShell για τη διαχείριση πόρων Azure, ανατρέξτε στο θέμα [χρήση του PowerShell Azure με τη διαχείριση πόρων Azure.](../powershell-azure-resource-manager.md)
- Για να μάθετε σχετικά με την εφαρμογή υπηρεσίας περιβάλλοντα, ανατρέξτε στο θέμα [Εισαγωγή στην εφαρμογή υπηρεσίας περιβάλλον.](app-service-app-service-environment-intro.md)
- Για να μάθετε σχετικά με τη Διαχείριση πιστοποιητικά SSL εφαρμογής υπηρεσίας με χρήση του PowerShell, ανατρέξτε στο θέμα [πιστοποιητικά SSL σύνδεση χρήση του PowerShell.](app-service-web-app-powershell-ssl-binding.md)
- Για να μάθετε σχετικά με την πλήρη λίστα των cmdlet του PowerShell που βασίζονται στο Azure από διαχειριστή πόρων για τις εφαρμογές Web Azure, ανατρέξτε στο θέμα [Azure αναφορές για τα Cmdlet του Web Apps Azure των cmdlet του PowerShell για τη διαχείριση πόρων.](https://msdn.microsoft.com/library/mt619237.aspx)
- - Για να μάθετε σχετικά με τη Διαχείριση εφαρμογών υπηρεσίας χρησιμοποιώντας CLI, ανατρέξτε στο θέμα [Using Azure Resource Manager-Based XPlat CLI για Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)
