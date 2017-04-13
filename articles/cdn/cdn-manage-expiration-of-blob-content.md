<properties
 pageTitle="Διαχείριση λήξης Azure χώρο αποθήκευσης αντικειμένων blob περιεχομένου στο Azure CDN | Microsoft Azure"
 description="Μάθετε περισσότερα σχετικά με τις επιλογές για τον έλεγχο time to live για αντικείμενα BLOB σε προσωρινή αποθήκευση Azure CDN."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Διαχείριση λήξης Azure χώρο αποθήκευσης αντικειμένων blob περιεχομένου στο Azure CDN

> [AZURE.SELECTOR]
- [Υπηρεσίες εφαρμογών/Cloud Azure Web, ASP.NET ή των υπηρεσιών IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure χώρο αποθήκευσης αντικειμένων blob υπηρεσίας](cdn-manage-expiration-of-blob-content.md)

Η [υπηρεσία αντικειμένων blob](../storage/storage-introduction.md#blob-storage) στο [Χώρο αποθήκευσης Azure](../storage/storage-introduction.md) είναι μία από πολλές προελεύσεις βασίζονται στο Azure ενσωματωμένο με το Azure CDN.  Οποιοδήποτε περιεχόμενο δυνατότητα δημόσιας πρόσβασης blob μπορεί να είναι προσωρινά στο Azure CDN μέχρι να περάσει το time to live (TTL).  Η τιμή TTL προσδιορίζεται από την [κεφαλίδα *Cache-Control* ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) στην απάντηση HTTP από το χώρο αποθήκευσης Azure.

>[AZURE.TIP] Μπορείτε να επιλέξετε να ορίσετε καμία TTL σε ένα αντικείμενο blob.  Σε αυτήν την περίπτωση, Azure CDN εφαρμόζει αυτόματα έναν προεπιλεγμένο TTL από επτά ημέρες.
>
>Για περισσότερες πληροφορίες σχετικά με τη λειτουργία Azure CDN ταχύτητα πρόσβαση σε αντικείμενα BLOB και άλλα αρχεία, ανατρέξτε στο θέμα η [Επισκόπηση CDN Azure](./cdn-overview.md).
>
>Για περισσότερες λεπτομέρειες σχετικά με την υπηρεσία Azure χώρο αποθήκευσης αντικειμένων blob, ανατρέξτε στο θέμα [Blob έννοιες εξυπηρέτησης](https://msdn.microsoft.com/library/dd179376.aspx). 

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει διάφορους τρόπους που μπορείτε να ορίσετε την τιμή TTL σε ένα αντικείμενο blob στο χώρο αποθήκευσης Azure.  

## <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell](../powershell-install-configure.md) είναι μόνο ένας από τους πιο γρήγορος, πιο ισχυρούς τρόπους για τη διαχείριση των υπηρεσιών Azure σας.  Χρησιμοποιήστε το `Get-AzureStorageBlob` cmdlet για να λάβετε μια αναφορά σε το αντικείμενο blob, στη συνέχεια, ορίστε το `.ICloudBlob.Properties.CacheControl` την ιδιότητα. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] Μπορείτε επίσης να χρησιμοποιήσετε το PowerShell για τη [Διαχείριση προφίλ CDN και τα τελικά σημεία](./cdn-manage-powershell.md).

## <a name="azure-storage-client-library-for-net"></a>Βιβλιοθήκη προγράμματος-πελάτη Azure χώρου αποθήκευσης για .NET

Για να ορίσετε ένα blob TTL χρησιμοποιώντας .NET, χρησιμοποιήστε τη [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για .NET](../storage/storage-dotnet-how-to-use-blobs.md) για να ορίσετε την ιδιότητα [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) .

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] Υπάρχουν πολλά περισσότερα δείγματα κώδικα .NET διαθέσιμες στα [Δείγματα χώρο αποθήκευσης αντικειμένων Blob Azure για το .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).

## <a name="other-methods"></a>Άλλες μέθοδοι

- [Azure περιβάλλον γραμμής εντολών](../xplat-cli-install.md)

    Κατά την αποστολή του blob, ορίστε την ιδιότητα *cacheControl* χρησιμοποιώντας το `-p` εναλλαγή.  Αυτό το παράδειγμα ορίζει την τιμή TTL σε μία ώρα (3600 δευτερόλεπτα).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Υπηρεσίες Azure αποθήκευσης REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    Ρητά ορίστε την ιδιότητα *x ms-blob-cache-στοιχείων ελέγχου* σε μια αίτηση [Τοποθέτηση αντικειμένων Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Τοποθέτηση μπλοκ λίστας](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)ή [Ορισμός ιδιοτήτων Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) .

- Εργαλεία διαχείρισης χώρου αποθήκευσης τρίτων κατασκευαστών

    Ορισμένα εργαλεία διαχείρισης χώρου αποθήκευσης Azure τρίτων κατασκευαστών σας επιτρέπει να ορίσετε την ιδιότητα *CacheControl* στα αντικείμενα blob. 

## <a name="testing-the-cache-control-header"></a>Δοκιμή της κεφαλίδας *Cache-Control*

Μπορείτε εύκολα να επαληθεύσετε την τιμή TTL της σας αντικείμενα blob.  Με χρήση του προγράμματος περιήγησής σας [Εργαλεία για προγραμματιστές](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), ελέγξτε ότι το blob είναι συμπεριλαμβανομένης της κεφαλίδας απόκρισης *Cache-Control* .  Μπορείτε επίσης να χρησιμοποιήσετε ένα εργαλείο όπως **wget**, [Postman](https://www.getpostman.com/)ή [Fiddler](http://www.telerik.com/fiddler) να εξετάσετε τις κεφαλίδες απόκρισης.

## <a name="next-steps"></a>Επόμενα βήματα

- [Διαβάστε σχετικά με την κεφαλίδα *Cache-Control*](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Μάθετε πώς να διαχειρίζεστε λήξης υπηρεσία Cloud περιεχομένου στο Azure CDN](./cdn-manage-expiration-of-cloud-service-content.md)

