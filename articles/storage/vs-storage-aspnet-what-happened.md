<properties
    pageTitle="Τι συνέβη με το έργο ASP.NET μου; | Microsoft Azure | Visual Studio συνδεδεμένες υπηρεσίες"
    description="Περιγράφει τι συμβαίνει αφού προσθέσετε χώρο αποθήκευσης Azure σε ένα έργο ASP.NET χρήση του Visual Studio συνδεδεμένες υπηρεσίες"
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

# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>Τι απέγινε το έργο μου ASP.NET (αποθήκευσης Azure Visual Studio συνδεδεμένοι υπηρεσίας);

## <a name="references-added"></a>Αναφορές που προσθέσατε

Το πακέτο NuGet χώρου αποθήκευσης Azure προστέθηκε στο έργο σας Visual Studio.  
Αυτό το πακέτο προσθέτει τις παρακάτω αναφορές .NET:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

##<a name="connection-string-for-azure-storage-added"></a>Συμβολοσειρά σύνδεσης για το χώρο αποθήκευσης Azure προστεθεί
Στο αρχείο web.config του έργου σας, ένα στοιχείο που δημιουργήθηκε με τη συμβολοσειρά σύνδεσης του λογαριασμού του επιλεγμένου χώρου αποθήκευσης και το κλειδί.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ASP.NET](http://www.asp.net).
