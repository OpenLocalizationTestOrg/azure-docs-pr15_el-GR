<properties
   pageTitle="Επαλήθευση σύνδεσης πύλης | Microsoft Azure"
   description="Αυτό το άρθρο παρουσιάζει τον τρόπο για να επιβεβαιώσετε μια σύνδεση πύλης στο μοντέλο ανάπτυξης για τη διαχείριση πόρων"
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
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Επαλήθευση σύνδεσης πύλης

Μπορείτε να επαληθεύσετε τη σύνδεσή σας πύλης με διαφορετικούς τρόπους. Σε αυτό το άρθρο θα σας δείξουν πώς μπορείτε να επαληθεύσετε την κατάσταση μιας σύνδεσης πύλης διαχείρισης πόρων, χρησιμοποιώντας την πύλη του Azure και με χρήση του PowerShell.


## <a name="verify-using-powershell"></a>Επαληθεύστε τη χρήση του PowerShell

Θα πρέπει να εγκαταστήσετε την πιο πρόσφατη έκδοση του τα cmdlet του PowerShell για τη διαχείριση πόρων Azure. Για πληροφορίες σχετικά με την εγκατάσταση των cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md). Για περισσότερες πληροφορίες σχετικά με τη χρήση των cmdlet διαχείρισης πόρων, ανατρέξτε στο θέμα [Χρήση του Windows PowerShell με το διαχειριστή πόρων](../powershell-azure-resource-manager.md).

### <a name="step-1-log-in-to-your-azure-account"></a>Βήμα 1: Σύνδεση στο λογαριασμό σας στο Azure

1. Ανοίξτε την κονσόλα του PowerShell σας με αναβαθμισμένα δικαιώματα και συνδεθείτε στο λογαριασμό σας.

        Login-AzureRmAccount

2. Ελέγξτε τις συνδρομές για το λογαριασμό.

        Get-AzureRmSubscription 

3. Καθορίστε τη συνδρομή στην οποία θέλετε να χρησιμοποιήσετε.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Βήμα 2: Επαληθεύστε τη σύνδεσή σας


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Επαλήθευση με την πύλη Azure

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Επόμενα βήματα

- Μπορείτε να προσθέσετε εικονικές μηχανές εικονικού δίκτυά σας. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

