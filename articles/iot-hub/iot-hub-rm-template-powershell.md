<properties
    pageTitle="Δημιουργήστε ένα διανομέα IoT χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure και PowerShell | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για να ξεκινήσετε με τη χρήση προτύπων Azure από διαχειριστή πόρων για να δημιουργήσετε ένα διανομέα IoT με το PowerShell."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Δημιουργήστε ένα διανομέα IoT χρήση του PowerShell

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Εισαγωγή

Μπορείτε να χρησιμοποιήσετε για τη διαχείριση πόρων Azure για να δημιουργήσετε και να διαχειριστείτε διανομείς Azure IoT μέσω προγραμματισμού. Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure για να δημιουργήσετε ένα διανομέα IoT με το PowerShell.

> [AZURE.NOTE] Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα για τη δημιουργία και εργασία με πόρους: [Διαχείριση πόρων Azure και κλασική](../resource-manager-deployment-model.md).  Σε αυτό το άρθρο καλύπτει χρησιμοποιώντας το μοντέλο ανάπτυξης Azure διαχείριση πόρων.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης χρειάζεστε τα εξής:

- Λογαριασμού Azure active. <br/>Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] ή νεότερη έκδοση.

> [AZURE.TIP] Στο άρθρο [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure] [ lnk-powershell-arm] παρέχει περισσότερες πληροφορίες σχετικά με τη χρήση δεσμών ενεργειών του PowerShell και διαχείριση πόρων Azure πρότυπα για να δημιουργήσετε Azure πόρους. 

## <a name="connect-to-your-azure-subscription"></a>Σύνδεση με τη συνδρομή σας στο Azure

Σε μια γραμμή εντολών του PowerShell, πληκτρολογήστε την παρακάτω εντολή για να εισέλθετε στο Azure τη συνδρομή σας:

```
Login-AzureRmAccount
```

Μπορείτε να χρησιμοποιήσετε τις παρακάτω εντολές για να ανακαλύψετε όπου μπορείτε να αναπτύξετε ένα διανομέα IoT και τις εκδόσεις API υποστηρίζονται αυτήν τη στιγμή:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Δημιουργήστε μια ομάδα πόρων ώστε να περιέχει το Κέντρο IoT σας χρησιμοποιώντας την ακόλουθη εντολή σε μία από τις υποστηριζόμενες θέσεις για IoT διανομέα. Αυτό το παράδειγμα δημιουργεί μια ομάδα πόρων που ονομάζεται **MyIoTRG1**:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Υποβολή ενός προτύπου για τη διαχείριση πόρων Azure για να δημιουργήσετε ένα διανομέα IoT

Χρησιμοποιήστε ένα πρότυπο JSON για να δημιουργήσετε ένα διανομέα IoT στο σας ομάδα πόρων. Μπορείτε επίσης να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure για να κάνετε αλλαγές σε ένα υπάρχον διανομέα IoT.

1. Χρησιμοποιήστε ένα πρόγραμμα επεξεργασίας κειμένου για να δημιουργήσετε ένα πρότυπο από διαχειριστή πόρων Azure που ονομάζεται **template.json** με τον ακόλουθο ορισμό πόρων για να δημιουργήσετε ένα νέο πρότυπο διανομέα IoT. Αυτό το παράδειγμα προσθέτει την ενότητα IoT στην περιοχή **Ανατολικής η.π.α.** , δημιουργεί δύο ομάδες καταναλωτή (**cg1** και **cg2**) στα το τελικό σημείο διανομέα συμβατό με το συμβάν και χρησιμοποιεί την έκδοση **2016-02-03** API. Αυτό το πρότυπο αναμένει επίσης για τη μεταβίβαση το όνομα διανομέας IoT ως παράμετρο, που ονομάζεται **hubName**. Για την τρέχουσα λίστα θέσεων που υποστηρίζουν διανομέα IoT δουν [Την κατάσταση Azure][lnk-status].

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
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
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

2. Αποθηκεύστε το αρχείο προτύπου για τη διαχείριση πόρων Azure στον τοπικό υπολογιστή σας. Αυτό το παράδειγμα προϋποθέτει αποθηκεύστε το σε ένα φάκελο που ονομάζεται **c:\templates**.

3. Εκτελέστε την παρακάτω εντολή για να αναπτύξετε το νέο διανομέα IoT, μεταβίβαση το όνομα του Κέντρο σας IoT ως παράμετρο. Σε αυτό το παράδειγμα, το όνομα του διανομέα IoT είναι **abcmyiothub** (Σημειώστε ότι αυτό το όνομα πρέπει να είναι μοναδικό καθολικό έτσι θα πρέπει να περιλαμβάνει το όνομα ή τα αρχικά):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. Το αποτέλεσμα εμφανίζει τα πλήκτρα για την ενότητα IoT που δημιουργήσατε.

5. Μπορείτε να επαληθεύσετε ότι η εφαρμογή σας προσθέσει ο νέος διανομέας IoT με την επίσκεψή σας στην [πύλη του Azure] [ lnk-azure-portal] και προβολή της λίστας των πόρων ή χρησιμοποιώντας το cmdlet **Get-AzureRmResource** PowerShell.

> [AZURE.NOTE] Αυτή η εφαρμογή παράδειγμα προσθέτει ένα διανομέα IoT τυπική S1 για την οποία θα γίνει χρέωση. Μπορείτε να διαγράψετε την ενότητα IoT μέσω της [πύλης Azure] [ lnk-azure-portal] ή χρησιμοποιώντας το cmdlet **Κατάργηση AzureRmResource** PowerShell, όταν είστε έτοιμοι.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε αναπτύξει ένα διανομέα IoT χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure με το PowerShell, ίσως θελήσετε να εξερευνήσετε περαιτέρω:

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
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
