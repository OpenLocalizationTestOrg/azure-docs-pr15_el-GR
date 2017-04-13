<properties
    pageTitle="Δημιουργήστε ένα διανομέα IoT χρησιμοποιώντας Azure CLI | Microsoft Azure"
    description="Ακολουθήστε αυτό το άρθρο για να δημιουργήσετε ένα διανομέα IoT, χρησιμοποιώντας το περιβάλλον γραμμής εντολών Azure."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Δημιουργήστε ένα διανομέα IoT χρησιμοποιώντας Azure CLI

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Εισαγωγή

Μπορείτε να χρησιμοποιήσετε Azure περιβάλλον γραμμής εντολών για να δημιουργήσετε και να διαχειριστείτε διανομείς Azure IoT μέσω προγραμματισμού. Αυτό το άρθρο σας δείχνει πώς μπορείτε να χρησιμοποιήσετε το CLI Azure για να δημιουργήσετε ένα διανομέα IoT.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης χρειάζεστε τα εξής:

- Λογαριασμού Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.
- [Azure CLI 0.10.4] [ lnk-CLI-install] ή νεότερη έκδοση. Εάν έχετε ήδη Azure CLI μπορείτε να επικυρώσετε την τρέχουσα έκδοση από τη γραμμή εντολών με την ακόλουθη εντολή:
```
    azure --version
```

> [AZURE.NOTE] Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα για τη δημιουργία και εργασία με πόρους: [Διαχείριση πόρων Azure και κλασική](../resource-manager-deployment-model.md). Το Azure CLI πρέπει να είναι σε λειτουργία Azure διαχείριση πόρων:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Ορίσετε λογαριασμός Azure και συνδρομή 

1. Κατά τη σύνδεση εντολών, πληκτρολογώντας την ακόλουθη εντολή
```
    azure login
```
Χρησιμοποιήστε το πρόγραμμα περιήγησης web προτεινόμενες και κώδικα για τον έλεγχο ταυτότητας.

2. Εάν έχετε πολλές συνδρομές Azure, τη σύνδεση στο Azure θα εκχωρήσετε πρόσβαση σε όλες τις συνδρομές Azure που σχετίζεται με τα διαπιστευτήριά σας. Μπορείτε να προβάλετε τις συνδρομές Azure, καθώς και ποια είναι η προεπιλεγμένη, χρησιμοποιώντας την εντολή
```
    azure account list 
```

Για να ρυθμίσετε το περιβάλλον συνδρομή στην οποία θέλετε να εκτελέσετε τα υπόλοιπα τη χρήση εντολών

```
    azure account set <subscription name>
```

3. Εάν δεν έχετε μια ομάδα πόρων, μπορείτε να δημιουργήσετε ένα επώνυμο **exampleResourceGroup** 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] Το άρθρο [Χρησιμοποιήστε το Azure CLI για τη Διαχείριση Azure τους πόρους και τις ομάδες πόρων] [ lnk-CLI-arm] παρέχει περισσότερες πληροφορίες σχετικά με τον τρόπο χρήσης του Azure CLI για τη Διαχείριση Azure πόρων. 


## <a name="create-an-iot-hub"></a>Δημιουργήστε ένα διανομέα IoT

Απαιτούμενες παράμετροι:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Για να δείτε όλες τις παραμέτρους διαθέσιμη για τη δημιουργία μπορείτε να χρησιμοποιήσετε την εντολή Βοήθεια στη γραμμή εντολών
```
    azure iothub create -h 
```
Γρήγορο παράδειγμα:

 Για να δημιουργήσετε ένα διανομέα IoT που ονομάζονται **exampleIoTHubName** με την ομάδα πόρων **exampleResourceGroup** απλώς εκτελέστε την ακόλουθη εντολή
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] Αυτή η εντολή Azure CLI δημιουργεί ένα διανομέα IoT τυπική S1 για την οποία θα γίνει χρέωση. Μπορείτε να διαγράψετε την ενότητα IoT **exampleIoTHubName** χρησιμοποιώντας παρακάτω εντολή 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Επόμενα βήματα
Για να μάθετε περισσότερα σχετικά με την ανάπτυξη IoT διανομέα, ανατρέξτε στα παρακάτω:
- [Ο διανομέας IoT SDK][lnk-sdks]

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Με την πύλη Azure για τη Διαχείριση IoT διανομέα][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
