<properties
    pageTitle="Τι απέγινε το έργο μου WebJob (αποθήκευσης Azure Visual Studio συνδεδεμένοι υπηρεσίας); | Microsoft Azure"
    description="Περιγράφει τι συνέβη σε ένα έργο Azure WebJob μετά τη σύνδεση με ένα λογαριασμό χώρου αποθήκευσης χρησιμοποιώντας το Visual Studio συνδεδεμένες υπηρεσίες"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Τι απέγινε το έργο μου WebJob (αποθήκευσης Azure Visual Studio συνδεδεμένοι υπηρεσίας);

## <a name="references-added"></a>Αναφορές που προσθέσατε

Το πακέτο NuGet χώρου αποθήκευσης Azure έχει προστεθεί ή να ενημερώνονται στο έργο σας Visual Studio.  
Αυτό το πακέτο προσθέτει τις παρακάτω αναφορές .NET:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Συμβολοσειρά σύνδεσης για το χώρο αποθήκευσης Azure προστεθεί
Στο αρχείο App.config του έργου σας, τις καταχωρήσεις **AzureWebJobsStorage** και **AzureWebJobsDashboard** ενημερώθηκαν με συμβολοσειρά σύνδεσης του λογαριασμού του επιλεγμένου χώρου αποθήκευσης και κλειδί.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Azure WebJobs τεκμηρίωση πόρους](http://go.microsoft.com/fwlink/?linkid=390226).
