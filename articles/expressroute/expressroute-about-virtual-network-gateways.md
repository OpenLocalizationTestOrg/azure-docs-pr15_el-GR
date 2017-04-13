<properties 
   pageTitle="Σχετικά με τις πύλες εικονικού δικτύου ExpressRoute | Microsoft Azure"
   description="Μάθετε σχετικά με τις πύλες εικονικού δικτύου για ExpressRoute."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>Σχετικά με τις πύλες εικονικού δικτύου για ExpressRoute


Μια πύλη εικονικού δικτύου χρησιμοποιείται για την αποστολή της κυκλοφορίας δικτύου μεταξύ Azure εικονικών δικτύων και εσωτερικής θέσεις. Όταν ρυθμίζετε τις παραμέτρους μιας σύνδεσης ExpressRoute, πρέπει να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους μιας πύλης εικονικού δικτύου και μια σύνδεση πύλης εικονικού δικτύου.

Όταν δημιουργείτε μια πύλη εικονικού δικτύου, μπορείτε να ορίσετε διάφορες ρυθμίσεις. Μία από τις απαιτούμενες ρυθμίσεις Καθορίζει εάν η πύλη θα χρησιμοποιηθούν για την κυκλοφορία ExpressRoute ή VPN τοποθεσίας σε τοποθεσία. Στο μοντέλο ανάπτυξης για τη διαχείριση πόρων, η ρύθμιση είναι '-GatewayType'.

Κατά την κίνηση του δικτύου αποστέλλεται σε μια αποκλειστική ιδιωτικής σύνδεσης, μπορείτε να χρησιμοποιήσετε τον τύπο πύλη 'ExpressRoute'. Αυτό είναι επίσης αναφέρεται ως πύλη ExpressRoute. Κατά την κίνηση του δικτύου αποστέλλεται κρυπτογραφημένα δημόσια στο Internet, μπορείτε να χρησιμοποιήσετε τον τύπο πύλη 'Vpn'. Αυτό αναφέρεται ως πύλη VPN. -Τοποθεσίας, σημείο σε τοποθεσία και συνδέσεις VNet-VNet όλα χρησιμοποιούν μια πύλη VPN. 

Κάθε εικονικού δικτύου μπορεί να έχει μόνο μία πύλη εικονικού δικτύου ανά τύπο πύλης. Για παράδειγμα, μπορείτε να έχετε μία εικονικού δικτύου πύλης που χρησιμοποιεί - GatewayType Vpn και ένα που χρησιμοποιεί - GatewayType ExpressRoute. Σε αυτό το άρθρο εστιάζει στην πύλη ExpressRoute εικονικού δικτύου.

## <a name="gwsku"></a>SKU πύλης

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Εάν θέλετε να αναβαθμίσετε την πύλη σε μια πιο ισχυρή πύλη SKU, στις περισσότερες περιπτώσεις μπορείτε να χρησιμοποιήσετε το cmdlet του PowerShell 'Αλλαγή μεγέθους-AzureRmVirtualNetworkGateway'. Αυτό θα λειτουργεί για αναβαθμίσεις σε τυπικές και HighPerformance SKU. Ωστόσο, για να αναβαθμίσετε για να την SKU UltraPerformance, θα πρέπει να αναδημιουργήσετε της πύλης.

###  <a name="aggthroughput"></a>Εκτιμώμενη συγκεντρωτικών αποτελεσμάτων μετάδοσης από πύλη SKU


Ο παρακάτω πίνακας εμφανίζει τους τύπους πύλης και την εκτιμώμενη απόδοση συγκεντρωτικών αποτελεσμάτων. Αυτός ο πίνακας ισχύει για το τόσο τη διαχείριση πόρων και τα μοντέλα κλασική ανάπτυξης.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>Cmdlet του REST API και PowerShell

Για επιπλέον πόρους τεχνική και σύνταξη συγκεκριμένες απαιτήσεις κατά τη χρήση REST API και cmdlet του PowerShell για ρυθμίσεις παραμέτρων πύλης εικονικού δικτύου, ανατρέξτε στο θέμα οι ακόλουθες σελίδες:

|**Κλασικό** | **Διαχείριση πόρων**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στο θέμα [Επισκόπηση ExpressRoute](expressroute-introduction.md) για περισσότερες πληροφορίες σχετικά με τις ρυθμίσεις παραμέτρων διαθέσιμη σύνδεση. 







 
