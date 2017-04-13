<properties
    pageTitle="Τι απέγινε το έργο μου ASP.NET 5 (Visual Studio συνδεδεμένοι υπηρεσιών) | Χώρος αποθήκευσης του Windows Azure"
    description="Περιγράφει τι συμβαίνει αφού τη σύνδεση με ένα λογαριασμό του Azure χώρου αποθήκευσης σε ένα έργο Visual Studio ASP.NET 5 χρησιμοποιώντας το Visual Studio συνδεδεμένες υπηρεσίες"
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

# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Τι απέγινε το έργο μου ASP.NET 5 (αποθήκευσης Azure Visual Studio συνδεδεμένοι υπηρεσιών);

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

Επίσης, το πακέτο NuGet **Microsoft.Framework.Configuration.Json** προστέθηκε.

## <a name="connection-string-for-azure-storage-added"></a>Συμβολοσειρά σύνδεσης για το χώρο αποθήκευσης Azure προσθέσει
Στο αρχείο config.json του έργου σας, ένα στοιχείο που δημιουργήθηκε με τη συμβολοσειρά σύνδεσης του λογαριασμού του επιλεγμένου χώρου αποθήκευσης και το κλειδί.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ASP.NET 5](http://www.asp.net/vnext).
