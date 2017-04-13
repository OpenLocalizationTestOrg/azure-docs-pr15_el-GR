<properties
   pageTitle="Τροποποίηση IP πύλης και στα προθέματα διεύθυνση IP του τοπικού δικτύου πύλης | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς καθοδηγεί αλλαγή προθέματα διεύθυνση IP για την πύλη τοπικού δικτύου"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>Τροποποίηση ρυθμίσεων πύλης τοπικό δίκτυο με χρήση του PowerShell

Ορισμένες φορές να αλλάξετε τις ρυθμίσεις για την πύλη τοπικό δίκτυο AddressPrefix ή GatewayIPAddress. Τις παρακάτω οδηγίες θα σας βοηθήσει να τροποποιήσετε τις ρυθμίσεις πύλης τοπικού δικτύου. Μπορείτε, επίσης, να τροποποιήσετε αυτές τις ρυθμίσεις στην πύλη του Azure.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε
    
Θα πρέπει να εγκαταστήσετε την πιο πρόσφατη έκδοση του τα cmdlet του PowerShell για τη διαχείριση πόρων Azure. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .

## <a name="to-modify-ip-address-prefixes"></a>Για να τροποποιήσετε προθέματα διευθύνσεων IP

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>Για να τροποποιήσετε τη διεύθυνση IP πύλης

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να επαληθεύσετε τη σύνδεσή σας πύλης. Ανατρέξτε στο θέμα [επαλήθευση σύνδεσης πύλης](vpn-gateway-verify-connection-resource-manager.md).

