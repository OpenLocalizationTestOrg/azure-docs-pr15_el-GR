<properties
    pageTitle="Δημιουργήστε ένα διανομέα IoT χρησιμοποιώντας ένα πρότυπο ARM και C# | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για να ξεκινήσετε με τη χρήση προτύπων Azure από διαχειριστή πόρων για να δημιουργήσετε ένα διανομέα IoT με ένα πρόγραμμα C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Δημιουργήστε ένα διανομέα IoT χρησιμοποιώντας ένα πρόγραμμα C# με ένα πρότυπο από διαχειριστή πόρων Azure

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Εισαγωγή

Μπορείτε να χρησιμοποιήσετε για τη διαχείριση πόρων Azure για να δημιουργήσετε και να διαχειριστείτε διανομείς Azure IoT μέσω προγραμματισμού. Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure για να δημιουργήσετε ένα διανομέα IoT από ένα πρόγραμμα C#.

> [AZURE.NOTE] Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα για τη δημιουργία και εργασία με πόρους: [Διαχείριση πόρων Azure και κλασική](../resource-manager-deployment-model.md).  Σε αυτό το άρθρο καλύπτει χρησιμοποιώντας το μοντέλο ανάπτυξης Azure διαχείριση πόρων.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

- Microsoft Visual Studio 2015.
- Λογαριασμού Azure active. <br/>Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.
- [Λογαριασμός αποθήκευσης Azure] [ lnk-storage-account] όπου μπορείτε να αποθηκεύσετε τα αρχεία σας από διαχειριστή πόρων Azure πρότυπο.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] ή νεότερη έκδοση.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Προετοιμασία το έργο Visual Studio

1. Στο Visual Studio, δημιουργήστε ένα έργο Visual C# Windows χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Το όνομα του έργου **CreateIoTHub**.

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο σας και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

3. Στο Nuget πακέτο Manager, επιλέξτε **περιλαμβάνουν προέκδοση**και αναζητήστε το **Microsoft.Azure.Management.ResourceManager**. Κάντε κλικ στην επιλογή **εγκατάσταση**, **Αναθεώρηση** αλλαγών, κάντε κλικ στο κουμπί **OK**και κατόπιν κάντε κλικ στην επιλογή **να αποδοχή** για να αποδεχτείτε τις άδειες χρήσης.

4. Στο Nuget διαχείρισης πακέτου, πραγματοποιήστε αναζήτηση για **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Κάντε κλικ στην επιλογή **εγκατάσταση**, **Αναθεώρηση** αλλαγών, κάντε κλικ στο κουμπί **OK**και, στη συνέχεια, κάντε κλικ στην επιλογή **να αποδοχή** για να αποδεχτείτε την άδεια χρήσης.

5. Στο Program.cs, αντικαταστήστε το υπάρχον δηλώσεις **χρησιμοποιώντας** με τα εξής:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. Στο Program.cs, προσθέστε τις ακόλουθες μεταβλητές στατική αντικαθιστώντας τις τιμές του πλαισίου κράτησης θέσης. Κάνατε μια Σημείωση **αναγνωριστικά εφαρμογής**, **SubscriptionId**, **TenantId**και **τον κωδικό πρόσβασης** σε αυτό το εκπαιδευτικό μάθημα. **Όνομα του λογαριασμού σας χώρο αποθήκευσης Azure** είναι το όνομα του λογαριασμού αποθήκευσης Azure όπου αποθηκεύετε τα αρχεία σας από διαχειριστή πόρων Azure πρότυπο. **Όνομα ομάδας πόρων** είναι το όνομα της ομάδας πόρων που χρησιμοποιείτε όταν δημιουργείτε την ενότητα IoT, μπορεί να είναι μια ήδη υπάρχουσα ομάδα πόρων ή ένα νέο. **Ανάπτυξη όνομα** είναι ένα όνομα για την ανάπτυξη, όπως **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Υποβολή ενός προτύπου για τη διαχείριση πόρων Azure για να δημιουργήσετε ένα διανομέα IoT

Χρησιμοποιήστε ένα αρχείο προτύπου και παράμετρο JSON για να δημιουργήσετε ένα διανομέα IoT στο σας ομάδα πόρων. Μπορείτε επίσης να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure για να κάνετε αλλαγές σε ένα υπάρχον διανομέα IoT.

1. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο σας, κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο στοιχείο**. Προσθήκη αρχείου JSON που ονομάζεται **template.json** στο έργο σας.

2. Αντικαταστήστε τα περιεχόμενα του **template.json** με τον ακόλουθο ορισμό πόρων για να προσθέσετε ένα τυπικό διανομέα IoT στην περιοχή **Ανατολικής ΗΠΑ** (για την τρέχουσα λίστα των περιοχών που υποστηρίζουν διανομέα IoT δουν [Την κατάσταση Azure][lnk-status]):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

3. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο σας, κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο στοιχείο**. Προσθήκη αρχείου JSON που ονομάζεται **parameters.json** στο έργο σας.

4. Αντικαταστήστε τα περιεχόμενα του **parameters.json** με τις ακόλουθες πληροφορίες παραμέτρου που ορίζει ένα όνομα για τη νέα ενότητα IoT όπως **{τα αρχικά σας} mynewiothub**. Σημειώστε ότι το όνομα διανομέας IoT πρέπει να είναι μοναδικό καθολικό έτσι θα πρέπει να περιλαμβάνει το όνομα ή τα αρχικά):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. Στην **Εξερεύνηση Server**, συνδεθείτε με τη συνδρομή σας στο Azure και στο λογαριασμό σας στο χώρο αποθήκευσης Azure δημιουργία κοντέινερ που ονομάζεται **πρότυπα**. Στο παράθυρο **ιδιοτήτων** , ορίστε τα δικαιώματα **δημόσια πρόσβαση για ανάγνωση** για το κοντέινερ **πρότυπα** για να **Blob**.

6. Στην **Εξερεύνηση Server**, κάντε δεξί κλικ στο κοντέινερ **πρότυπα** και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή Blob κοντέινερ**. Κάντε κλικ στο κουμπί **Αποστολή Blob** , επιλέξτε τα δύο αρχεία, **parameters.json** και **templates.json**και, στη συνέχεια, κάντε κλικ στην επιλογή **Άνοιγμα** για να αποστείλετε τα αρχεία JSON στο κοντέινερ **πρότυπα** . Τις διευθύνσεις URL των τα αντικείμενα BLOB που περιέχει τα δεδομένα JSON είναι οι εξής:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. Προσθέστε την ακόλουθη μέθοδο Program.cs:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. Προσθέστε τον ακόλουθο κώδικα στη μέθοδο **CreateIoTHub** για να υποβάλετε τα αρχεία προτύπου και παραμέτρου για τη διαχείριση πόρων Azure:

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. Προσθέστε τον ακόλουθο κώδικα στη μέθοδο **CreateIoTHub** που εμφανίζει την κατάσταση και τα πλήκτρα για τη νέα ενότητα IoT:

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Ολοκλήρωση και εκτελέστε την εφαρμογή

Μπορείτε τώρα να ολοκληρώσετε την εφαρμογή, καλώντας τη μέθοδο **CreateIoTHub** πριν δημιουργήσετε και εκτελέστε το.

1. Προσθέστε τον ακόλουθο κώδικα στο τέλος της μεθόδου **κύριες** :

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Κάντε κλικ στην επιλογή **Δημιουργία** και, στη συνέχεια, να **δημιουργήσετε λύση**. Διορθώστε τυχόν σφάλματα.

3. Κάντε κλικ στην επιλογή **Εντοπισμός σφαλμάτων** και, στη συνέχεια, **Ξεκινήστε τον εντοπισμό σφαλμάτων** για να εκτελέσετε την εφαρμογή. Ενδέχεται να χρειαστούν αρκετά λεπτά για την ανάπτυξη για να εκτελέσετε.

4. Μπορείτε να επαληθεύσετε ότι η εφαρμογή σας προσθέσει ο νέος διανομέας IoT με την επίσκεψή σας στην [πύλη του Azure] [ lnk-azure-portal] και προβολή της λίστας των πόρων ή χρησιμοποιώντας το cmdlet **Get-AzureRmResource** PowerShell.

> [AZURE.NOTE] Αυτή η εφαρμογή παράδειγμα προσθέτει ένα διανομέα IoT τυπική S1 για την οποία θα γίνει χρέωση. Μπορείτε να διαγράψετε την ενότητα IoT μέσω της [πύλης Azure] [ lnk-azure-portal] ή χρησιμοποιώντας το cmdlet **Κατάργηση AzureRmResource** PowerShell, όταν είστε έτοιμοι.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε αναπτύξει ένα διανομέα IoT χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure με ένα πρόγραμμα C#, που μπορεί να θέλετε να εξερευνήσετε περαιτέρω:

- Διαβάστε σχετικά με τις δυνατότητες της [υπηρεσίας παροχής πόρων διανομέα IoT REST API][lnk-rest-api].
- Διαβάστε την [Επισκόπηση της διαχείρισης πόρων Azure] [ lnk-azure-rm-overview] για να μάθετε περισσότερα σχετικά με τις δυνατότητες της διαχείρισης πόρων Azure.

Για να μάθετε περισσότερα σχετικά με την ανάπτυξη IoT διανομέα, ανατρέξτε στα παρακάτω:

- [Εισαγωγή στις C SDK][lnk-c-sdk]
- [Ο διανομέας IoT SDK][lnk-sdks]

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Προσομοίωση μια συσκευή με το SDK IoT πύλης][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
