<properties
   pageTitle="Δημιουργία κοντέινερ υπηρεσία cloud με το PowerShell | Microsoft Azure"
   description="Σε αυτό το άρθρο εξηγεί πώς μπορείτε να δημιουργήσετε ένα κοντέινερ υπηρεσία cloud με το PowerShell. Το κοντέινερ φιλοξενεί ρόλους web και εργασίας."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Χρησιμοποιήστε μια εντολή Azure PowerShell για να δημιουργήσετε ένα κοντέινερ υπηρεσιών cloud κενή
Σε αυτό το άρθρο εξηγεί τον τρόπο για να δημιουργήσετε γρήγορα ένα κοντέινερ Cloud Services με χρήση των cmdlet του Azure PowerShell. Ακολουθήστε τα παρακάτω βήματα:

1. Εγκαταστήστε το Microsoft Azure PowerShell cmdlet από τη σελίδα [λήψεων του Azure PowerShell](http://aka.ms/webpi-azps) .
2. Ανοίξτε τη γραμμή εντολών του PowerShell.
3. Χρησιμοποιήστε το [Πρόσθετο AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) για να συνδεθείτε.

    > [AZURE.NOTE] Για περαιτέρω οδηγίες σχετικά με την εγκατάσταση το cmdlet του Azure PowerShell και τη σύνδεση με τη συνδρομή σας στο Azure, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

4. Χρησιμοποιήστε το cmdlet **New-AzureService** για να δημιουργήσετε μια κενή Azure cloud κοντέινερ υπηρεσιών.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. Ακολουθήστε αυτό το παράδειγμα για να καλέσετε το cmdlet:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία την υπηρεσία Azure cloud, εκτελέστε:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Επόμενα βήματα

 * Για να διαχειριστείτε την ανάπτυξη υπηρεσία cloud, ανατρέξτε στις εντολές [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Κατάργηση AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)και [Σύνολο AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) . Μπορείτε επίσης μπορεί να ανατρέξετε στο θέμα [Πώς να ρυθμίσετε τις υπηρεσίες cloud](cloud-services-how-to-configure.md) για περαιτέρω πληροφορίες.

 * Για να δημοσιεύσετε το έργο υπηρεσία cloud Azure, ανατρέξτε στο δείγμα κώδικα **PublishCloudService.ps1** από [συνεχής παράδοσης για την υπηρεσία cloud στο Azure](cloud-services-dotnet-continuous-delivery.md).
