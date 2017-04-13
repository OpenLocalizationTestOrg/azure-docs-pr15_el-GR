<properties
    pageTitle="Ρύθμιση του αριθμού-κλειδιού θάλαμο για εικονικές μηχανές στη Διαχείριση πόρων Azure | Microsoft Azure"
    description="Πώς μπορείτε να ρυθμίσετε θάλαμο αριθμού-κλειδιού για χρήση με ένα διαχειριστή πόρων Azure εικονική μηχανή."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Ρύθμιση του αριθμού-κλειδιού θάλαμο για εικονικές μηχανές στη Διαχείριση πόρων Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης

Στη Διαχείριση πόρων Azure στοίβα, απόρρητο/πιστοποιητικά είναι διαμορφωθεί ως πόρους που παρέχονται από την υπηρεσία παροχής πόρων του αριθμού-κλειδιού θάλαμο. Για να μάθετε περισσότερα σχετικά με το κλειδί θάλαμο, ανατρέξτε στο θέμα [Τι είναι το Azure κλειδί θάλαμο;](../key-vault/key-vault-whatis.md)

>[AZURE.NOTE] 
>
>1. Προκειμένου θάλαμο αριθμού-κλειδιού για να χρησιμοποιηθεί με το διαχειριστή πόρων Azure εικονικές μηχανές, πρέπει να ορίσετε την ιδιότητα **EnabledForDeployment** στον θάλαμο αριθμού-κλειδιού στην τιμή true. Μπορείτε να το κάνετε στα διάφορα προγράμματα-πελάτες.
>
>2. Το κλειδί θάλαμο πρέπει να δημιουργηθεί στην ίδια συνδρομή και θέση ως η εικονική μηχανή.

## <a name="use-powershell-to-set-up-key-vault"></a>Χρήση του PowerShell για τη ρύθμιση του αριθμού-κλειδιού θάλαμο
Για να δημιουργήσετε ένα πλήκτρο θάλαμο με χρήση του PowerShell, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure κλειδί θάλαμο](../key-vault/key-vault-get-started.md#vault).

Για το νέο κλειδί χώροι φύλαξης, μπορείτε να χρησιμοποιήσετε αυτό το cmdlet του PowerShell:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Για υπάρχοντα κλειδιού χώροι φύλαξης, μπορείτε να χρησιμοποιήσετε αυτό το cmdlet του PowerShell:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>Μας CLI για να ρυθμίσετε θάλαμο αριθμού-κλειδιού
Για να δημιουργήσετε ένα πλήκτρο θάλαμο χρησιμοποιώντας το περιβάλλον γραμμής εντολών (CLI), ανατρέξτε στο θέμα [Διαχείριση θάλαμο κλειδί με χρήση CLI](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

Για CLI, πρέπει να δημιουργήσετε το κλειδί θάλαμο μπορέσετε να αντιστοιχίσετε την πολιτική ανάπτυξης. Μπορείτε να το κάνετε αυτό, χρησιμοποιώντας την ακόλουθη εντολή:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Χρησιμοποιήστε πρότυπα για να ρυθμίσετε θάλαμο αριθμού-κλειδιού
Ενώ μπορείτε να χρησιμοποιήσετε ένα πρότυπο, πρέπει να ορίσετε το `enabledForDeployment` ιδιότητα που θα `true` για τον πόρο θάλαμο αριθμού-κλειδιού.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Για άλλες επιλογές που μπορείτε να ρυθμίσετε κατά τη δημιουργία ενός κλειδιού θάλαμο με τη χρήση προτύπων, ανατρέξτε στο θέμα [Δημιουργία ενός κλειδιού θάλαμο](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
