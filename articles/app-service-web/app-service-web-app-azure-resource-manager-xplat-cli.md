<properties
    pageTitle="Εργαλεία Azure γραμμής εντολών για τη διαχείριση πόρων βάσει πλατφόρμες για Azure στο Web App | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε τα νέα εργαλεία γραμμής εντολών από διαχειριστή πόρων Azure βάσει πλατφόρμες για τη Διαχείριση εφαρμογών Web της Azure."
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

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Χρήση του Azure πόρων βάσει Manager XPlat CLI για Azure Web App#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Με την έκδοση του Microsoft Azure εργαλεία της γραμμής εντολών πλατφόρμες έκδοση 0.10.5, έχουν προστεθεί νέες εντολές. Αυτές οι εντολές δίνει στο χρήστη τη δυνατότητα να χρησιμοποιήσετε τις εντολές του PowerShell που βασίζονται στο Azure από διαχειριστή πόρων για τη Διαχείριση εφαρμογών Web.

Για να μάθετε σχετικά με τη διαχείριση ομάδων πόρων, ανατρέξτε στο θέμα [χρήση του Azure CLI για τη Διαχείριση Azure τους πόρους και τις ομάδες πόρων](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Διαχείριση εφαρμογών υπηρεσίας προγράμματος ##

### <a name="create-an-app-service-plan"></a>Δημιουργήστε ένα πρόγραμμα εφαρμογής υπηρεσίας ###
Για να δημιουργήσετε ένα σχέδιο παροχής υπηρεσιών εφαρμογής, χρησιμοποιήστε την εντολή **Δημιουργία azure appserviceplan** .

Ακολουθούν περιγραφές από τις διαφορετικές παραμέτρους:

-   **--ομάδα πόρων**: ομάδα πόρων που περιλαμβάνει το πρόγραμμα υπηρεσιών που έχουν δημιουργηθεί πρόσφατα εφαρμογής.
-   **--όνομα**: όνομα εφαρμογή το σχέδιο παροχής υπηρεσιών.
-   **--θέση**: εφαρμογή υπηρεσίας πρόγραμμα θέση.
-   **--επίπεδο**: την επιθυμητή τιμολόγησης sku (οι επιλογές είναι: F1 (δωρεάν). D1 (κοινόχρηστο). B1 (βασικές μικρό), B2 (βασικές Μεσαίο), και B3 (Basic μεγάλο). S1 (τυπική μικρό), S2 (τυπική Μεσαίο), και S3 (τυπική μεγάλο). P1 (μικρό Premium), P2 (Μεσαίο Premium), και P3 (Premium για μεγάλο).)
-   **--παρουσίες**: ο αριθμός των εργαζομένων στην εφαρμογή υπηρεσίας σχέδιο (η προεπιλεγμένη τιμή είναι 1).

Παράδειγμα για να χρησιμοποιήσετε αυτό το cmdlet:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Λίστα υπάρχοντα σχέδια εφαρμογής υπηρεσίας ###

Για να παραθέσετε τα υπάρχοντα προγράμματα υπηρεσίας εφαρμογής, χρησιμοποιήστε εντολή **azure appserviceplan λίστας** .

Για να δημιουργήσετε μια λίστα με όλα τα προγράμματα υπηρεσίας εφαρμογής στην περιοχή μιας συγκεκριμένης ομάδας πόρων, χρησιμοποιήστε:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Για να λάβετε μια συγκεκριμένη εφαρμογή σχέδιο παροχής υπηρεσιών, χρησιμοποιήστε εντολή **Εμφάνιση azure appserviceplan** :

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Ρύθμιση παραμέτρων ενός υπάρχοντος σχεδίου υπηρεσίας εφαρμογής ###

Για να αλλάξετε τις ρυθμίσεις για ένα υπάρχον σχέδιο της εφαρμογής υπηρεσίας, χρησιμοποιήστε την εντολή **azure appserviceplan config** . Μπορείτε να αλλάξετε την sku και τον αριθμό των εργαζομένων 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Κλιμάκωση ένα πρόγραμμα εφαρμογής υπηρεσίας ####

Για να περιορίσετε το μέγεθος ενός υπάρχοντος σχεδίου υπηρεσίας εφαρμογής, χρησιμοποιήστε:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Αλλάζοντας το SKU από ένα πρόγραμμα εφαρμογής υπηρεσίας ####

Για να αλλάξετε την sku από ένα υπάρχον πρόγραμμα υπηρεσίας εφαρμογής, χρησιμοποιήστε:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Διαγραφή ενός υπάρχοντος σχεδίου υπηρεσίας εφαρμογής ###

Για να διαγράψετε ένα υπάρχον σχέδιο της εφαρμογής υπηρεσίας, όλες οι εφαρμογές web που του έχουν ανατεθεί πρέπει να είναι μετακινήθηκε ή διαγράφηκε πρώτα. Στη συνέχεια, χρησιμοποιώντας την εντολή **Διαγραφή azure webapp** μπορείτε να διαγράψετε το σχέδιο παροχής υπηρεσιών εφαρμογής.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>Διαχείριση εφαρμογών Web εφαρμογής υπηρεσίας ##

### <a name="create-a-web-app"></a>Δημιουργία εφαρμογής Web ###

Για να δημιουργήσετε μια εφαρμογή web, χρησιμοποιήστε την εντολή **Δημιουργία azure webapp** .

Ακολουθούν περιγραφές από τις διαφορετικές παραμέτρους:

- **--όνομα**: όνομα για την εφαρμογή web.
- **--πρόγραμμα**: όνομα για το πρόγραμμα υπηρεσιών που χρησιμοποιείται για τη φιλοξενία της εφαρμογής web.
- **--ομάδα πόρων**: ομάδα πόρων που φιλοξενεί το πρόγραμμα υπηρεσιών εφαρμογής.
- **--θέση**: τη θέση της εφαρμογής web.

Παράδειγμα για να χρησιμοποιήσετε αυτό το cmdlet:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Διαγράψτε μια υπάρχουσα εφαρμογή Web ###

Για να διαγράψετε μια υπάρχουσα εφαρμογή web, μπορείτε να χρησιμοποιήσετε την εντολή **Διαγραφή azure webapp** , πρέπει να καθορίσετε το όνομα του web app και το όνομα της ομάδας πόρων.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>Εφαρμογές Web της υπάρχουσας λίστας ###

Για να δημιουργήσετε μια λίστα με τις υπάρχουσες εφαρμογές web, χρησιμοποιήστε την εντολή **azure webapp λίστας** .

Για να δημιουργήσετε μια λίστα με όλες τις εφαρμογές web στην περιοχή μιας συγκεκριμένης ομάδας πόρων, χρησιμοποιήστε:

    azure webapp list --resource-group ContosoAzureResourceGroup

Για να λάβετε μια εφαρμογή web συγκεκριμένες, χρησιμοποιήστε την εντολή **Εμφάνιση azure webapp** .

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Ρύθμιση παραμέτρων σε μια υπάρχουσα εφαρμογή Web ###

Για να αλλάξετε τις ρυθμίσεις και ρυθμίσεις παραμέτρων για μια υπάρχουσα εφαρμογή web, χρησιμοποιήστε την εντολή **Ορισμός azure webapp config** .

Παράδειγμα (1): Αλλάξτε την έκδοση php από μια εφαρμογή web 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Παράδειγμα (2): Προσθήκη ή αλλαγή των ρυθμίσεων της εφαρμογής

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Να γνωρίζετε τι μπορεί να είναι άλλα ρύθμισης παραμέτρων αλλάξει, χρησιμοποιήστε την εντολή **config azure webapp set -h** .

### <a name="change-the-state-of-an-existing-web-app"></a>Να αλλάξετε την κατάσταση από μια υπάρχουσα εφαρμογή Web ###

#### <a name="restart-a-web-app"></a>Επανεκκινήστε μια εφαρμογή web ####

Για να ξεκινήσετε ξανά μια εφαρμογή web, πρέπει να καθορίσετε την ομάδα όνομα και πόρων της εφαρμογής web.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Διακοπή μια εφαρμογή web ####

Για να διακόψετε μια εφαρμογή web, πρέπει να καθορίσετε την ομάδα όνομα και πόρων της εφαρμογής web.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Ξεκινήστε μια εφαρμογή web ####

Για να ξεκινήσετε μια εφαρμογή web, πρέπει να καθορίσετε την ομάδα όνομα και πόρων της εφαρμογής web.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Διαχείριση προφίλ εφαρμογή δημοσίευσης στο Web ###

Κάθε εφαρμογή web έχει ένα προφίλ δημοσίευσης που μπορούν να χρησιμοποιηθούν για να δημοσιεύσετε τις εφαρμογές σας.

#### <a name="get-publishing-profile"></a>Λήψη δημοσίευσης προφίλ ####

Για να λάβετε το προφίλ δημοσίευσης για μια εφαρμογή web, χρησιμοποιήστε:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Αυτή η εντολή αντηχήσεων το δημοσίευσης όνομα προφίλ χρήστη και τον κωδικό πρόσβασης στη γραμμή εντολών.

### <a name="manage-web-app-hostnames"></a>Διαχείριση ονόματα Web App ###

Για να διαχειριστείτε συνδέσεις όνομα κεντρικού υπολογιστή για την εφαρμογή web σας, χρησιμοποιήστε την εντολή **azure webapp config ονόματα**  

#### <a name="list-hostname-bindings"></a>Λίστα συνδέσεων όνομα κεντρικού υπολογιστή ####

Για να λάβετε την τρέχουσα συνδέσεις όνομα κεντρικού υπολογιστή για μια εφαρμογή web, χρησιμοποιήστε:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Προσθήκη συνδέσεων όνομα κεντρικού υπολογιστή ####

Για να προσθέσετε συνδέσεις όνομα κεντρικού υπολογιστή σε μια εφαρμογή web, χρησιμοποιήστε:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Διαγραφή συνδέσεις όνομα κεντρικού υπολογιστή ####

Για να διαγράψετε συνδέσεις όνομα κεντρικού υπολογιστή, χρησιμοποιήστε:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Επόμενα βήματα ###
- Για να μάθετε σχετικά με την υποστήριξη Azure CLI διαχείριση πόρων, ανατρέξτε στο θέμα [Χρησιμοποιήστε το CLI Azure για να διαχειριστείτε Azure τους πόρους και τις ομάδες πόρων.](../xplat-cli-azure-resource-manager.md)
- Για να μάθετε σχετικά με τη Διαχείριση εφαρμογών υπηρεσίας χρησιμοποιώντας το PowerShell, ανατρέξτε στο θέμα [Using Azure Resource Manager-Based PowerShell για τη Διαχείριση εφαρμογών Web Azure.](app-service-web-app-azure-resource-manager-powershell.md)
